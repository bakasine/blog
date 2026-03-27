---
title: codex-console 工具
date: 2026-03-27 19:54:44
tags:
  - ai
  - tool
categories:
  - note
---

- [__一. 安装__](#install)
- [__二. 配置__](#config)
- [__三. 中转站__](#relay)
- [__四. 定时任务__](#cron)
- [__五. 备份__](#backup)

# <h2 id="install" style="color:#FF8C00">安装</h2>

项目地址：<https://github.com/dou-jiang/codex-console>

这里更推荐直接使用 Release 中已经发布好的可执行文件。

原因也很简单：虽然 Docker Compose 用起来方便，但每次更新通常都要重新编译，更新流程不够直接，想升级版本时也不如直接替换可执行文件省事。

不过这篇文章里还是演示一下 Docker Compose 的安装方式，步骤非常简单：

```bash
git clone https://github.com/dou-jiang/codex-console.git
cd codex-console
docker compose up -d
```

执行完成后，服务就已经启动起来了，后面再继续进行配置即可。

# <h2 id="config" style="color:#FF8C00">配置</h2>

安装完成后，先访问：

```text
ip:1455
```

系统默认密码是：

```text
admin123
```

第一次登录之后，强烈建议第一时间把默认密码改掉。

修改路径也很直接：点击 `设置` -> `访问控制`，然后修改密码即可。

# <h2 id="relay" style="color:#FF8C00">中转站</h2>

在 `设置` -> `上传` 里，可以配置 CPA、sub2api 等中转站。

这里以 CPA 为例：

1. 点击 `添加服务`
2. 在 `API URL` 中填写：`ip:8317`
3. 在 `API Token` 中填写 CPA 的 `secret-key`
4. 点击测试，确认是否能够正常连通

如果测试通过，就说明 codex-console 已经可以通过 CPA 进行中转了。

# <h2 id="cron" style="color:#FF8C00">定时任务</h2>

`crontab -e`

```bash
*/30 * * * * /root/register.sh
```

`这个是简单的50次批量注册的请求, 注册成功后上传到cpa`

```bash
curl 'http://ip:1455/api/registration/batch' \
  -H 'Accept: */*' \
  -H 'Accept-Language: zh-CN,zh;q=0.9' \
  -H 'Connection: keep-alive' \
  -H 'Content-Type: application/json' \
  -b 'webui_auth=通过F12抓取' \
  -H 'Origin: http://ip:1455' \
  -H 'Referer: http://ip:1455/' \
  -H 'User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/146.0.0.0 Safari/537.36' \
  --data-raw '{"email_service_type":"tempmail","auto_upload_cpa":true,"cpa_service_ids":[1],"auto_upload_sub2api":false,"sub2api_service_ids":[],"auto_upload_tm":false,"tm_service_ids":[],"count":50,"interval_min":5,"interval_max":30,"concurrency":3,"mode":"pipeline"}' \
  --insecure
```

# <h2 id="backup" style="color:#FF8C00">备份</h2>

`建议定时备份防止出现异常情况, 尤其是docker compose安装的方式`