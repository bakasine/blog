# 准备工作

```
1.下载 nodejs 安装
https://nodejs.org/en
// 指定版本16.10版本可以用这个链接
https://nodejs.org/download/release/v16.10.0/node-v16.10.0-win-x64.zip

2.安装git
https://github.com/git-for-windows/git/releases/download/v2.39.2.windows.1/Git-2.39.2-64-bit.exe
```

# Usage

``` bash
git clone git@github.com:bakahentailolicon/blog.git ./blog

git clone git@github.com:bakahentailolicon/gal-theme.git ./blog/themes/gal-theme

cd blog

npm install -g mirror-config-china --registry=https://registry.npmmirror.com

npm i

npm install --save hexo-renderer-sass-next

npm un hexo-renderer-marked --save
npm i hexo-renderer-markdown-it --save

```

# Hexo

[简单使用教程](https://bakahentailolicon.github.io/2018/05/26/hello-world/)

``` bash
// 编写文章, 所有文章都在 blog\source\_posts 下
hexo new 文件名

// 写完文章后需要运行该命令编译成前端文件
hexo g

// 本地运行 访问 localhost:4000 浏览博客效果
hexo s

// 推送到 github 如果你配置了 git page 
hexo d
````

# gal-theme可能存在的问题

1. 新本版node的npm版本太高，sass不能支持，最高只能使用版本7的npm。可以下载v16.10的node。

``` bash
// 版本回退命令 
npm install npm@7.20.0 -g
```
	
2. 国内网络会导致下载各种依赖出问题通过一下命令修改源

``` bash
// 下载国外的资源众所周知的慢，常用设置镜像，平时多用yarn
//  yarn全局安装及设置镜像

npm install -g yarn
yarn config set registry http://registry.npm.taobao.org/ -g
yarn config set sass_binary_site http://cdn.npm.taobao.org/dist/node-sass -g
npm config set registry https://registry.npm.taobao.org

// 查看是否配置成功
npm config get registry 

 // 查看npm当前配置
npm config list

// 强制清除缓存
npm cache clear --force 

// 然后再安装gal-theme的依赖
yarn add hexo-renderer-sass
yarn add hexo-renderer-scss
cnpm install hexo-generator-json-content --save


// 如果 hexo g 后 sass 还是报错可以改用 cnpm 安装
cnpm install node-sass

```