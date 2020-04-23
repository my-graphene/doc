# MyGChain composition

MyGChain is composed of three main parts: `core`、`ui`、`faucet`：

![](core.png)

## core

MyGChain public chain is developed based on Graphene technology and via C++ language.

core is composed of `Core models of the chain` and `Command line wallet` programs.

- `Core models of the chain` —— full nodes, namely the witness_node program, connect to MyGChain network by P2P approach, receive the newest block from the network and broadcast the locally signed transaction package to the network. [witness_node parameter introduction] (node/cmd/witness_node.md)  
- `Command line wallet` —— namely the cli_wallet program, connects to witness_node via websocket, manages wallet files; provides transaction signature functionality, broadcasts to the public once the transaction is signed; provides API through http rpc for other programs to call. [cli_wallet parameter introduction] (node/cmd/cli_wallet.md)

Please refer to [Establish private chain] (node/private-chain.md) for establishing private chains.

## ui

`web online wallet` —— block browser, also the online wallet. The surfer client in web format, developed based on Nodejs.

## faucet

Faucet, used to connect to web wallet client, register accounts on the chain. Developed based on ruby.
