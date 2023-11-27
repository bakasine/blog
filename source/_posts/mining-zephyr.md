---
title: Zephyr 挖矿
date: 2023-11-27 14:49:36
tags:
    - zephyr
    - note
categories:
    - mining
---

* [工具下载](#tools)
* [矿池](#miningpools)
* [配置文件](#config)
* [查看数据](#data)
* [优化](#optimization)
* [问题集](#qa)

# <h2 id="tools">工具下载</h2>

`x86有编译好的版本(带捐赠)`

**[xmrig](https://github.com/xmrig/xmrig/releases)**

`arm手动编译`

**[编译流程](https://xmrig.com/docs/miner/build/ubuntu)**

__1.下载源码__

```
#需要自己编译
apt-get install git build-essential cmake automake libtool autoconf -y
git clone https://github.com/xmrig/xmrig.git
```
	
__2.去掉1%抽水,编辑 src/donate.h，将以下的数值改成0__

```
kMinimumDonateLevel=0
kDefaultDonateLevel=0
```
 
__3.编辑 src/net/strategies/DonateStrategy.cpp__

**将里面的kDonateHost和kDonateHostTls改成自己的代理地址，如果没有修改，可以改成127.0.0.1。**

__4.编译__

```
mkdir xmrig/build && cd xmrig/scripts
./build_deps.sh && cd ../build
cmake .. -DXMRIG_DEPS=scripts/deps
make -j$(nproc)
```

# <h2 id="miningpools">矿池</h2>

**[miningpools](https://miningpoolstats.stream/zephyr)**


# <h2 id="config">配置文件</h2>

`以miningocean为例`

```
"algo": null,改为"algo": "RandomX",
"url": "donate.v2.xmrig.com:3333",改为"url": "hk-zephyr.miningocean.org:5432",
"user": "YOUR_WALLET_ADDRESS",改为你的mexc钱包地址
"tls": false,改为"tls": true,
```

`编写systemd文件`

```
cat > /etc/systemd/system/xmrig.service << EOF
[Unit]
Description=miner service
[Service]
ExecStart=/root/xmrig --config=/root/config.json
CPUQuota=80%
Restart=always
Nice=10
CPUWeight=1
[Install]
WantedBy=multi-user.target
EOF
```

`开机自启`

```
systemctl enable xmrig
```

`开始运行`

```
systemctl start xmrig
```

`查看状态`

```
journalctl -fu xmrig
```

`关闭自启`

```
systemctl disable xmrig
```

# <h2 id="data" style="color:#FF8C00">查看数据</h2>

`统计数据和付款历史`

**[挖矿数据](https://zephyr.miningocean.org/worker_stats)**

# <h2 id="optimization" style="color:#FF8C00">优化</h2>

`启用hugepages，算力提升20-30%，会占用2.5GB内存`

```
bash -c "echo vm.nr_hugepages=1280 >> /etc/sysctl.conf"
```

# <h2 id="qa" style="color:#FF8C00">问题集</h2>

# <h3 id="v6only">服务器只有ipv6</h3>

```
cp /etc/resolv.conf /etc/resolv.conf.bak
echo -e "nameserver 2a01:4f8:c2c:123f::1\nnameserver 2a00:1098:2c::1\nnameserver 2a01:4f9:c010:3f02::1" > /etc/resolv.conf
```