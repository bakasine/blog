---
title: 门罗币类钱包挖矿使用指北
date: 2024-02-12 15:25:37
tags: 
    - monero
categories: 
    - mining
---

____

```
./WALLET --start-mining ADDRESS --mining-threads "$(nproc)" --detach
```

__运行__

```
# 在后台运行守护进程
./monerod --detach

# 查看日志文件
tail -f ~/.bitmonero/bitmonero.log

# 要求守护进程退出
./monerod exit

# 停止挖矿
./monerod stop_mining

# 恢复钱包
./mangonote-wallet-cli --restore-from-seed
```


