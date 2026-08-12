---
title: Claude 相关
date: 2026-03-27 20:48:06
tags:
  - ai
  - claude
  - cli
categories:
  - ai
---

- [__配置第三方 API__](#third_party_api)
  + [__1. CLI__](#claude_cli)
  + [__2. VS Code__](#claude_vscode)
- [__跳过首次登陆__](#skip_first_login)

# <h2 id="third_party_api">配置第三方 API</h2>

# <h3 id="claude_cli">1. CLI</h3>

```bash
export ANTHROPIC_BASE_URL=ip:8317
export ANTHROPIC_AUTH_TOKEN=apikey
export ANTHROPIC_MODEL='opusplan' # opusplan模式 或者直接填写模型名

export ANTHROPIC_DEFAULT_OPUS_MODEL='model'
export ANTHROPIC_DEFAULT_SONNET_MODEL='model'
export ANTHROPIC_DEFAULT_HAIKU_MODEL="$ANTHROPIC_DEFAULT_SONNET_MODEL"
export CLAUDE_CODE_SUBAGENT_MODEL="$ANTHROPIC_DEFAULT_SONNET_MODEL"
```

# <h3 id="claude_vscode">2. VS Code</h3>

```json
"claudeCode.preferredLocation": "panel",
"claudeCode.disableLoginPrompt": true,
"claudeCode.selectedModel": "gpt-5.4",
"claudeCode.environmentVariables": [
  {
    "name": "ANTHROPIC_BASE_URL",
    "value": "http://ip:8317"
  },
  {
    "name": "ANTHROPIC_AUTH_TOKEN",
    "value": "apikey"
  },
  {
    "name": "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC",
    "value": "1"
  },
  {
    "name": "ANTHROPIC_MODEL",
    "value": "gpt-5.4"
  }
]
```

# <h2 id="skip_first_login">跳过首次登陆</h2>

在 `~/.claude.json` 配置文件里添加以下字段：

```json
{
  "hasCompletedOnboarding": true
}
```