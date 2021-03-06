## 智能合约介绍

MyGChain智能合约2.0，底层使用WebAssembly虚拟机，目前支持C++语言的智能合约编写。
开发者使用C++编写智能合约，通过llvm将代码编译成WebAssembly（又名WASM），部署到区块链上，通过智能合约ABI(Application Binary Interface，应用程序的二进制接口)和智能合约交互。

# 合约帐户设计
  1. 帐户分2种：普通帐户和合约帐户
  2. 合约帐户，由普通帐户通过部署合约的方式创建，合约帐户无私钥，资产权限由合约代码控制
  3. 合约帐户对象，code字段存储合约代码, abi存储合约代码和abi
  4. 合约代码不支持更新

# 合约的持久化存储
  1. 定义合约的数据存储格式； 定义线性存储空间，每个元素是一个struct类型
  2. witness_node运行时，合约的持久化存储，放内存；程序退出时，写入磁盘；后期可以考虑优化内存(磁盘+缓存)。
  3. 封装读写持久化存储的API，  提供读、写、检索方法的接口供合约调用
  4. 合约只能写自己的存储空间，可以读其它合约的存储空间，但不可直接写；  同一合约内不同帐户的存储空间，由用户合约代码控制权限
  5. 合约内可以根据区块号获取区块，读取外部状态

# 智能合约公共库，供智能合约在虚拟机内部调用
 1. 提供API，获取head block num,  获取head block id
 2. get_balance 获取外部帐户余额
 3. 加解密API、验签API
 4. 持久化存储API

# 销毁合约(暂不支持)
合约内可以提供销毁方法，如：
selfdestruct(string account) 将资产转至recipient帐户，然后销毁合约
回收的存储空间，按全局手续费参数计算，奖励给合约创建者(从系统资金池中获取,  最多返还存储价格的20%)。

# 合约调用合约(暂不支持)
目前只能由普通帐户和合约帐户交互，不支持合约间交互。

# 技术实现
1. 合约代码： 暂时支持C++，后续会支持更多语言
2. 编译工具：llvm + binaryen， 将C++代码编译成wasm代码
3. 智能合约执行环境： WebAssembly作为底层执行VM，VM调用封装的API读取外部状态、读写外部存储。 wasm-jit 地址：https://github.com/WebAssembly/wasm-jit-prototype

