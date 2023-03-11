---
title: 搭建 AI 语音 TTS 服务
date: 2023-02-26 02:44:26
tags:
    - ai
    - vits
categories: 
    - tool
---

[__安装__](#env)
[__常见问题__](#qa)

### <h2 id="env">安装</h2>

__1.安装git__

__2.安装 pip,python >= 3.7__

__3.安装 Microsoft C++ 生成工具__

[下载地址](https://visualstudio.microsoft.com/zh-hans/visual-cpp-build-tools/)

![1](ai-voice.png)

添加环境变量(根据自己安装的目录修改)

`C:\Program Files (x86)\Microsoft Visual Studio\2022\BuildTools\Common7\IDE\CommonExtensions\Microsoft\CMake\CMake\bin`

`C:\Program Files (x86)\Microsoft Visual Studio\2022\BuildTools\VC\Tools\MSVC\14.34.31933\bin\Hostx86\x64`

`win+r` > `输入cmd回车`

``` bash
pip install --upgrade pip -i https://mirrors.aliyun.com/pypi/simple

git lfs install
# 包含近1g的训练模型慢慢等吧
git clone https://huggingface.co/spaces/sayashi/vits-uma-genshin-honkai

cd vits-uma-genshin-honkai

pip install -r requirements.txt -i https://mirrors.aliyun.com/pypi/simple

# 启动服务
python app.py
```

### <h2 id="qa">常见问题</h2>

**启动服务显示 initialization of _internal failed**

numpy高版本bug,回退到1.23.5后正常

`pip install numpy==1.23.5 -i https://mirrors.aliyun.com/pypi/simple`

__pyopenjtalk模块安装失败__

`查看Microsoft C++ 生成工具是否有安装和环境变量是否正确`