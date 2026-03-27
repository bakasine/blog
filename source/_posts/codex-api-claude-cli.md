---
title: 通过 CPA 中转 Codex API 在Claude Cli上使用
date: 2026-03-27 19:28:33
tags:
  - ai
categories:
  - note
---

- [__一. 工具准备__](#prepare)
  + [__1. CPA__](#prepare_cpa)
  + [__2. Claude CLI__](#prepare_claude)
- [__二. CPA 配置__](#cpa_config)
  + [__1. 登录管理页面__](#cpa_login)
  + [__2. 配置认证信息__](#cpa_auth)
  + [__3. 准备 Claude 的配置__](#claude_env)

# <h2 id="prepare" style="color:#FF8C00">工具准备</h2>

在开始之前，先准备好 CPA 和 Claude CLI。

# <h3 id="prepare_cpa">1. CPA</h3>

项目地址：<https://github.com/router-for-me/CLIProxyAPI>

这里推荐直接使用 Docker Compose 方式部署，流程会比较简单：

```bash
git clone https://github.com/router-for-me/CLIProxyAPI.git
cd CLIProxyAPI

# 根据仓库中的示例文件，准备 .env 和 config.yaml
# 然后修改 remote-management 下的 allow-remote 和 secret-key

docker compose up -d
```

简单来说就是：先把项目拉下来，然后复制并准备 `.env` 和 `config.yaml` 这两个配置文件，接着修改 `remote-management` 下的 `allow-remote` 与 `secret-key`，完成后就可以直接用 Docker Compose 启动。

# <h3 id="prepare_claude">2. Claude CLI</h3>

Claude CLI 可以直接参考官方安装文档：<https://code.claude.com/docs/en/getting-started>

先把这两个工具准备好，后面的配置就会顺畅很多。

# <h2 id="cpa_config" style="color:#FF8C00">CPA 配置</h2>

CPA 安装完成后，接下来就是进入管理页面完成配置。

# <h3 id="cpa_login">1. 登录管理页面</h3>

浏览器访问：

```text
ip:8317/management.html
```

打开之后，输入前面在 `remote-management` 中设置的 `secret-key` 登录即可。

# <h3 id="cpa_auth">2. 配置认证信息</h3>

进入管理页面后，在`认证配置`里把 `API 密钥`改成你自己的即可，这一步主要是让后面的请求能够正确转发。

# <h3 id="claude_env">3. 准备 Claude 的配置</h3>

这里可以参考文档：<https://help.router-for.me/agent-client/claude-code.html>

要在 Claude CLI 上使用 CPA，核心就是准备这 4 个参数：

```bash
export ANTHROPIC_BASE_URL=ip:8317
export ANTHROPIC_AUTH_TOKEN=apikey
export CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1
export ANTHROPIC_MODEL=gpt-5.4
```

这几个参数的作用可以简单理解为：

- `ANTHROPIC_BASE_URL`：指定 CPA 的服务地址
- `ANTHROPIC_AUTH_TOKEN`：填写你在 CPA 认证配置中设置的 API 密钥
- `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1`：减少一些非必要流量
- `ANTHROPIC_MODEL`：指定实际要使用的模型

你可以选择每次运行 Claude CLI 之前先执行一次这些 `export` 命令，也可以把它们写进 `~/.bashrc`，这样以后就不用重复输入了。
