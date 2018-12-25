# RuiChain构成

RuiChain主要有`rui-core`、`rui-ui`、`rui-faucet`三大部分组成：

![](rui-core.png)

## <b>rui-core</b>

RuiChain公链，基于石墨烯技术，基于C++开发。

rui-core主要由`链核心模块`和`命令行钱包`程序组成。

- `链核心模块` —— 全节点，即witness_node程序， 通过 P2P 方式连接到RuiChain网络，从网络接收最新区块，向网络广播本地签署的交易包。[witness_node 参数介绍](node/cmd/witness_node.md) 
<br>
- `命令行钱包` —— 即cli_wallet程序， 通过 websocket 方式连接到 witness_node， 管理钱包文件； 提供交易签名功能，签名后通过 witness_node 向外广播； 通过 http rpc 的方式提供 API 供其他程序调用。[cli_wallet 参数介绍](node/cmd/cli_wallet.md)

搭建私链请查阅[搭建私链](node/private-chain.md)。

## <b>rui-ui</b>

`web在线钱包` —— 区块浏览器，同时也是在线的钱包。web形式的浏览器客户端，基于Nodejs开发。

## <b>rui-faucet</b>

水龙头，用于web钱包客户端连接，用以注册链上账户使用。它基于ruby开发。