# RuiChain composition

RuiChain is composed of three main parts: `rui-core`、`rui-ui`、`rui-faucet`：

![](rui-core.png)

## <b>rui-core</b>

RuiChain public chain is developed based on Graphene technology and via C++ language.

rui-core is composed of `Core models of the chain` and `Command line wallet` programs.

- `Core models of the chain` —— full nodes, namely the witness_node program, connect to RuiChain network by P2P approach, receive the newest block from the network and broadcast the locally signed transaction package to the network. [witness_node parameter introduction] (node/cmd/witness_node.md) 
<br>
- `Command line wallet` —— namely the cli_wallet program, connects to witness_node via websocket, manages wallet files; provides transaction signature functionality, broadcasts to the public once the transaction is signed; provides API through http rpc for other programs to call. [cli_wallet parameter introduction] (node/cmd/cli_wallet.md)

Please refer to [Establish private chain] (node/private-chain.md) for establishing private chains.

## <b>rui-ui</b>

`web online wallet` —— block browser, also the online wallet. The surfer client in web format, developed based on Nodejs.

## <b>rui-faucet</b>

Faucet, used to connect to web wallet client, register accounts on the chain. Developed based on ruby.
