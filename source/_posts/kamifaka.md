---
title: 搭建自动发卡平台
date: 2023-03-10 01:40:50
tags:
    - web
categories: 
    - note
---

[__一.安装环境__](#install)
[__二.安装发卡平台__](#faka)
[__三.修改密码__](#changepw)
[__四.收款方式__](#ali)
[__五.修改logo__](#logo)


# <h2 id="install">一.安装环境</h2>

`docker`

```bash
apt-get update
apt-get install docker-ce docker-ce-cli containerd.io
systemctl enable docker
systemctl start docker
```

`openssl`

```bash
apt-get install libssl-dev
```

# <h2 id="faka">二.安装发卡平台</h2>

```bash

# 不使用mysql,并发差
docker run -d --name=kmfaka -p 8000:8000 --restart=always -v /opt/kamifaka:/usr/src/app/public baiyuetribe/kamifaka

# 使用mysql
docker run -d \
    -p 8000:8000 \
    --restart=always \
    --name=kmfaka \
    -e DB_TYPE=Mysql \
    -e DB_HOST=数据库ip地址或容器地址"172.17.0.1" \
    -e DB_PORT=数据库端口 \
    -e DB_USER=数据库用户名 \
    -e DB_PASSWORD=数据库用密码 \
    -e DB_DATABASE=数据库名 \
    -v /opt/kamifaka:/usr/src/app/public \
    baiyuetribe/kamifaka
```

# <h2 id="changepw">三.修改密码</h2>

`访问:8000/admin` -> 默认账号`admin@qq.com`, 默认密码`123456` -> `用户修改` -> `立即修改`

# <h2 id="ali">四.收款方式</h2>

___开通当面付___

1.[web](https://b.alipay.com/signing/productDetailV2.htm?productId=I1011000290000001003)
2.手机支付宝搜索`当面付`

___填写相关资料___

1.经营类目 选择 “百货零售 / 其他零售 / 杂货店”，或者其他...问题不大
2.营业执照 可不上传
3.店铺招牌 可以拍一下身份的百货店，或者百度找一张类似的图

___配置密钥___

`开发设置` -> `接口加签方式（证书/密钥）` ->`生成rsa密钥`

``` bash
openssl
genrsa -out app_private_key.pem   2048 
pkcs8 -topk8 -inform PEM -in app_private_key.pem -outform PEM -nocrypt -out app_private_key_pkcs8.pem 
rsa -in app_private_key.pem -pubout -out app_public_key.pem 
exit
```

___app_public_key.pem内容填写___

得到ali公钥,保存并填入即可使用

# <h2 id="logo">五.修改logo</h2>

把你的logo文件替换掉`/opt/kamifaka`下的logo.png