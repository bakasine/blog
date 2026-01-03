---
title: WordPress 安装 Zibll 主题
date: 2026-01-4 02:44:26
tags:
    - wordpress
categories: 
    - note
---

[__安装 WordPress__](#ins)
[__创建伪授权脚本__](#fake)
[__上传主题并注入补丁__](#patch)
[__(可选) 安装 Caddy 通过反向代理__](#caddy)

# <h2 id="ins">安装 WordPress</h2>

```yaml
services:
  db:
    # We use a mariadb image which supports both amd64 & arm64 architecture
    image: mariadb:10.6.4-focal
    # If you really want to use MySQL, uncomment the following line
    #image: mysql:8.0.27
    command: '--default-authentication-plugin=mysql_native_password'
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=somewordpress
      - MYSQL_DATABASE=wordpress
      - MYSQL_USER=wordpress
      - MYSQL_PASSWORD=wordpress
    expose:
      - 3306
      - 33060
    networks:
      - wp-network
  wordpress:
    image: wordpress:latest
    ports:
      - 80:80
    # 核心修改 1：劫持 api.zibll.com 到容器自身
    extra_hosts:
      - "api.zibll.com:127.0.0.1"
      - "www.zibll.com:127.0.0.1"
    restart: always
    environment:
      - WORDPRESS_DB_HOST=db
      - WORDPRESS_DB_USER=wordpress
      - WORDPRESS_DB_PASSWORD=wordpress
      - WORDPRESS_DB_NAME=wordpress
    volumes:
      - ./web:/var/www/html
      # 核心修改 2：将伪授权脚本映射到容器路径
      - ./fake-api/api/auth:/var/www/html/api/auth
      - ./fake-api/api/auth:/var/www/html/api/update
    networks:
      - wp-network
volumes:
  db_data:
networks:
  wp-network:
    name: wp-network
    driver: bridge
```

# <h2 id="fake">创建伪授权脚本</h2>

**compose文件所在目录下创建该路径的文件**`fake-api/api/auth/index.php`

```php
<?php
$url = $_SERVER['REQUEST_URI'];

function getRandom($length) {
	$characters = 'abcdefghijklmnopqrstuvwxyz1234567890';
	$randomString = '';
	for ($i = 0; $i < $length; $i++) {
		$index = rand(0, strlen($characters) - 1);
		$randomString .= $characters[$index];
	}
	return $randomString;
}
function generate_randstr($url) {
	$key = strrev(md5($url));
	$num1 = rand(70,99);
	$num1r = strrev(strval($num1));
	$num2 = rand(70,99);
	$num2r = strrev(strval($num2));
	$key = substr($key,23).substr($key,0,23);
	$keystr = substr_replace($key,getRandom(3),$num1-69,0);
	$randstr = getRandom(3).$num1r.getRandom(rand(5,10)).$keystr.getRandom(100-$num2).$num2r;
	return $randstr;
}

header('Content-Type: application/json; charset=UTF-8');

if(strpos($url, '/api/auth') !== false){
	$time = time();
	$token = md5(uniqid(mt_rand(), true) . microtime());
	$randstr = generate_randstr($_POST['url']);
	$sign = md5($randstr.$time.$token.'ok');
    $data = ['error'=>true, 'error_code'=>0, 'msg'=>'', 'time'=>$time, 'token'=>$token, 'randstr'=>$randstr, 'code'=>base64_encode('恭喜您，授权验证成功'), 'sign'=>$sign];
    echo json_encode($data);
}
elseif(strpos($url, '/api/update') !== false){
    $version = $_POST['version'];
    $data = ['result'=>false, 'aut_error'=>false, 'msg'=>'暂无更新，您当前的版本已是最新版', 'version'=>$version];
    echo serialize($data);
}
```

# <h2 id="patch">上传主题并注入补丁</h2>

[__下载主题__](/file/zibll.zip)

* __(可选) 如果主题上传失败__

```bash
docker exec -i wordpress-wordpress-1 bash <<'EOF'
cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini
sed -i 's/upload_max_filesize = 2M/upload_max_filesize = 20M/g' /usr/local/etc/php/php.ini
sed -i 's/post_max_size = 8M/post_max_size = 25M/g' /usr/local/etc/php/php.ini
EOF
```

* __注入补丁__

在`web/wp-content/themes/zibll/functions.php`__顶部添加下列补丁__

```php
add_filter('pre_http_request', function($pre, $args, $url) {
    if (strpos($url, 'api.zibll.com') !== false) {
        $url = str_replace('https://', 'http://', $url);
        $args['sslverify'] = false;
        return wp_remote_request($url, $args);
    }
    return $pre;
}, 10, 3);
add_filter('pre_http_request', function($pre, $args, $url) {
    if (strpos($url, 'www.zibll.com') !== false) {
        $url = str_replace('https://', 'http://', $url);
        $args['sslverify'] = false;
        return wp_remote_request($url, $args);
    }
    return $pre;
}, 10, 3);
add_filter('https_ssl_verify', '__return_false');
add_filter('https_local_ssl_verify', '__return_false');
```

* __修改容器内的 Apache 劫持配置__

```bash
docker exec -i wordpress-wordpress-1 bash <<'EOF'
# 1. 开启 Rewrite 模块
a2enmod rewrite

# 2. 覆盖配置
cat <<EOC > /etc/apache2/sites-enabled/000-default.conf
<VirtualHost *:80>
    ServerName localhost
    DocumentRoot /var/www/html
</VirtualHost>

<VirtualHost *:80>
    ServerName api.zibll.com
    DocumentRoot /var/www/html
    
    # 关键：禁止自动补全目录斜杠，防止 301
    DirectorySlash Off
    RewriteEngine On
    
    # 只要是请求 api/auth 或 api/update，统统转发给 index.php 且不准重定向
    RewriteRule ^/api/auth/?$ /api/auth/index.php [L]
    RewriteRule ^/api/update/?$ /api/auth/index.php [L]

    <Directory /var/www/html/api/auth>
        Options -Indexes +FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
EOC

# 3. 重启
service apache2 restart
EOF
```

* __(可选) 清理 WordPress 数据库中的“重试限制”__

```bash
docker exec -i wordpress-db-1 mysql -uwordpress -pwordpress wordpress <<'EOF'
DELETE FROM wp_options WHERE option_name LIKE '_transient_zibll_auth%';
DELETE FROM wp_options WHERE option_name LIKE 'zibll_auth%';
EOF
```

# <h2 id="caddy">(可选) 安装 Caddy 通过反向代理</h2>

```yaml
services:
  caddy:
    image: caddy:latest
    container_name: caddy-server
    restart: unless-stopped
    ports:
      - "443:443"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./data:/data
      - ./config:/config
    networks:
      - wp-network

networks:
  wp-network:
    external: true
```

* 编写 Caddyfile 文件

```
你的域名 {
    # 自动 HTTPS 证书申请

    # 1. 开启压缩（提升加载速度，减少卡顿）
    encode gzip zstd

    # 2. 静态文件缓存优化（吸收了你提供的配置优点）
    # 注意：反代模式下，这些头部会附加在从 WordPress 传回的资源上
    @static {
        path_regexp static \.(?:css|js|woff2?|svg|gif|png|jpg|webp|jpeg|mp4|mp3|ico)$
    }
    header @static {
        Cache-Control "public, max-age=15778463"
        X-Content-Type-Options "nosniff"
    }

    # 3. 安全过滤（禁止访问敏感文件）
    @disallowed {
        path *.sql
        path /wp-content/uploads/*.php
        path /wp-content/debug.log
        path /xmlrpc.php
    }
    respond @disallowed 403

    # 4. 核心反代逻辑
    reverse_proxy wordpress-wordpress-1:80 {
        # 强制协议识别，解决后台卡顿和重定向循环
        header_up Host {host}
        header_up X-Real-IP {remote_host}
        header_up X-Forwarded-For {remote_host}
        header_up X-Forwarded-Proto https

        # 解决大文件上传进度条卡死
        flush_interval -1
    }

    # 5. 上传限制（配合子比主题 7M+ 的需求）
    request_body {
        max_size 128MB
    }

    # 错误日志记录（方便排查为何卡顿）
    log {
        output file /data/access.log
    }
}
```