---
title: 发行自己的加密货币并上架去中心交易所
date: 2023-06-05 19:58:19
tags:
  - note
  - crypto
categories: 
  - others
---

## 前  言 


**很多人都听过defi项目，也在uniswap或pancake上买过新币。uniswap与pancakeswap这种去中心化的平台其实每个人都可以成为自主的买家和卖家，发行自己的代币放上到平台进行交易，下面教程就是教大家怎么去部署一个自己的加密货币**


## 一、发币准备(所需工具及代码)：

[Chrome](https://www.google.com/chrome/)
[MetaMask](https://metamask.io/)
[Remix](http://remix.ethereum.org)

## 二、合约部署发币:

__1. 打开Remix__

`open remix` -> `create new file token.sol` -> `paste all code`

![](https://telegraph-image-d1y.pages.dev/file/c4d00e0490e6e3b2f42fe.png)

__2. 编译__

`check all parameters` -> `compiles`

![](https://telegraph-image-d1y.pages.dev/file/4b18321bef5171d7fbd18.png)

__3. 链接钱包__

`injected provider - metamask` -> `connect` -> `must choose ACprotocol` -> `transact`

![](https://telegraph-image-d1y.pages.dev/file/7e713dfa879a2709cbc41.png)

__4. 添加代币__

`copy token` -> `add coins`

![](https://telegraph-image-d1y.pages.dev/file/034e961bc37b54069d571.png)
![](https://telegraph-image-d1y.pages.dev/file/f6732117075fe6777ec57.png)

## 三、上架去中心化交易所

* ETH链:  [UniSwap](https://app.uniswap.org/)
* BSC链:  [PancakeSwap](https://pancakeswap.finance/swap)

__1.打开流动池页面，并连接 MetaMask 钱包，按下图操作：__

![](https://telegraph-image-d1y.pages.dev/file/0b02d8c3cf06606f78709.png)

__2.按下图：点击 创建币对__

![](https://telegraph-image-d1y.pages.dev/file/a451985ed8b1140efd002.png)

__3.第一个币选择所在公链原生代币(ETH链选ETH，BSC链选BNB)，也可以使用USDT，但还是推荐使用ETH或BNB效果较好，第二个币点击 选择代币 --> 粘贴新币的合约地址 --> 导入 --> 导入，按下图步骤操作__

![](https://telegraph-image-d1y.pages.dev/file/e9e2a08fd27011e8b7c51.png)

__4. 分别设置注入流动池的 ETH 与 新币 的数量比例，点击供应 --> 确认数量比例，点击 创建流动池和供应流动资金  -->  小狐狸钱包会弹出支付框，核对ETH数量与手续费后，点击确认， 按以下图示操作：（注意：流动池比例需自己计算好，比例决定新币初始价格，且初次注入流动性后，比例无法再次调整的，以后只能按这个比例随时增加减少或撤销流动池的币，如果要更改比例只能重新发一个币)__

![](https://telegraph-image-d1y.pages.dev/file/ea718a56860612deff725.png)

## 四、撤销流动池

__撤销流动池后，所有币都会回流到你自己的钱包(包括上架时添加的价值币、别人买新币花费的价值币及剩余的新币)，按下图步骤操作：__

![](https://telegraph-image-d1y.pages.dev/file/12346f60f48d11246a4ad.png)
![](https://telegraph-image-d1y.pages.dev/file/4bbe235e2b259196bee94.png)

## 五、开源教程

__到 [ETH link](https://etherscan.io/), [BSC link](https://bscscan.com/)  -->   粘贴代币合约地址 -->   搜索，如下图:__

![](https://telegraph-image-d1y.pages.dev/file/6a2174d3abc7017ea75fd.png)
![](https://telegraph-image-d1y.pages.dev/file/6b9b81c8dcde70bf4f907.png)
![](https://telegraph-image-d1y.pages.dev/file/84740c4c0e497d6f2e5c7.png)
![](https://telegraph-image-d1y.pages.dev/file/4b68fe339ffa44a94d01e.png)
![](https://telegraph-image-d1y.pages.dev/file/e9056134bc87a73448c0d.png)

## 六、参考

[Telegra.ph](https://telegra.ph/%E8%9C%9C%E7%BD%90%E5%90%88%E7%BA%A6%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B-Honeypot-Contract-Code-Guide-06-05)