
### 3. 启动见证节点witness_node， 同步数据

进入节点目录，启动MyGrapheneChain见证节点witness_node

```
# 可以使用2个参数，节省内存： --track-account 和 --partial-operations=true
nohup ./programs/witness_node/witness_node --data-dir=trusted_node --rpc-endpoint=127.0.0.1:38090 \
--p2p-endpoint=0.0.0.0:38091 --log-file --track-account "\"1.2.2999\""  --track-account "\"1.2.3000\"" \
--partial-operations=true >>witness.out 2>&1 &

# 输出日志文件，若无此参数，日志输出到控制台
--log-file 

```

目前全节点程序占用内存12GB+，运行时使用上--track-account account_id\(此处为1.2.x格式的帐户id\)和--partial-operations=true参数，内存中只保存交易所帐户的交易历史，内存可以控制在4GB内。

```



### 4. 运行命令行钱包cli_wallet

命令行钱包cli_wallet连接witness_node:

```
./programs/cli_wallet/cli_wallet -s ws://127.0.0.1:38090 \
--enable-rpc-log -r 127.0.0.1:38091 --data-dir=trusted_node
```

cli_wallet启动参数：

```
# 连接见证人节点的websocket rpc地址
-s

# 输出rpc日志文件
--enable-rpc-log

# cli_wallet提供的的websocket rpc地址, 开启cli_wallet的 API 服务
# 注意：不要配置为0.0.0.0，因为所有主机都可以访问你的钱包。
-r 127.0.0.1:38091

# 运行在守护进程模式
-d 

& 表示程序后台运行
```

首先需要为钱包设置一个钱包密码\(这个密码是本地的，用来解锁钱包\)：

```
new >>> set_password my_password
# 执行成功后会显示:
locked >>>
# 然后解锁钱包:
locked >>> unlock my_password
# 解锁成功会显示：
unlocked >>>
```

使用 info 命令可以查看当前区块同步情况

```
unlocked >>> info
info
{
  "head_block_num": 3913758,
  "head_block_id": "003bb81eec2abfdb2cf58ffdf4dd547ea190530e",
  "head_block_age": "3 seconds old",
  "next_maintenance_time": "0 second ago",
  "chain_id": "4f7d07969c446f8342033acb3ab2ae5044cbe0fde93db02de75bd17fa8fd84b8",
  "participation": "100.00000000000000000",
  ...
 }
 # head_block_age表示最新的区块时间，系统每3秒出一块
 # participation表示见证人参与率，见证人参与率必须大于70，网络才是正常的
```


### 5. 使用命令行钱包cli_wallet, 导入帐户私钥

如果你还没有帐户，需要先下载MyGrapheneChain轻钱包，或者访问在线钱包，注册帐号\(记得备份，保存好私钥\)。



### 6. 后台运行cli_wallet

在导入钱包私钥后，ctrl + c退出，此时会生成本地的钱包文件。再重新启动，启动时带上参数 -d &， 如下：

```
nohup ./programs/cli_wallet/cli_wallet -s ws://127.0.0.1:38090 \
--enable-rpc-log -r 127.0.0.1:38091 -d >>wallet.out 2>1 &
```

上述命令启动后，需要再多敲一次回车，然后在shell中输入exit来退出终端。相关的控制台输出被重定向到wallet.out文件。

## 四、 查询账户的交易历史， 监听用户充值

#### 通过账户查询账户的交易历史，以及获取交易的txID

cli_wallet不仅提供了命令行接口，还提供了json rpc接口。 钱包开启了 http rpc 方式的 API 服务时，效果与在钱包里输入命令相同。可以过wscat或者使用http客户端\(如curl\)来调用。

其中method 传入命令名，params 数组传入参数清单\(无参数时，params传空数组\)， id为请求的标识，返回结果中的id和请求id一致。如果执行成功，结果会有 result ，否则会有 error

cli_wallet提供了3个查询交易历史的接口：get_account_history, get_relative_account_history和get_account_history_by_operations。其中get_account_history_by_operations可以返回交易id\(txID\)。3个接口在[wallet api说明文档](https://doc.rui.io/core/ming-ling-xing-qian-bao-cli-wallet-api-shuo-ming.html)里都有说明。以get_account_history_by_operations为例，步骤如下：

1. 解锁钱包。
2. 根据帐户名，查询到帐户id。
3. 根据帐户id查询交易历史。

**详细过程如下，以帐户myuser为例**：

#### 1. 解锁钱包:

###### request：

```
// unlock解锁钱包，其中my_password为解锁密码
curl --data '{"jsonrpc": "2.0", "method": "unlock", "params": ["my_password"], "id": 1}' http://127.0.0.1:38091/rpc
```

解锁成功，返回：

```
{"id":1,"jsonrpc":"2.0","result":null}
```

#### 2. 获取myuser的帐户id:

若已知帐户id，可以跳过此步骤。

###### request：

```
// get_account_id，传入参数为帐户名或帐户id
curl --data '{"jsonrpc": "2.0", "method": "get_account_id", "params": ["myuser"], "id": 1}' http://127.0.0.1:38091/rpc
```

返回结果如下，其中帐户id为1.2.3054, "1.2."表示类型是账户：

```
{"id":1,"result":"1.2.3054"}
```

#### 3. 调用钱包的get_account_history_by_operations接口， 查询帐户交易历史，接口返回信息包含txID: 

若没有调用unlock解锁钱包，则查询出的转帐交易memo是无法解密的。此处，只有交易双方才能解密memo。

###### request：

```
// get_account_history_by_operations 第1个参数为帐户id，第2个参数为operation数组，可以传入空[]， 第3个参数为起始序号， 第4个参数为limit，表示获取最近limit笔交易历史
curl --data '{"jsonrpc": "2.0", "method": "get_account_history_by_operations", "params": ["1.2.3054",[], 1, 10], "id": 1}' http://127.0.0.1:38091/rpc
```

###### response：

```
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "total_without_operations": 3, // 此数字一般情况下等于传入的limit，表示被operation数组过滤之前，实际查询到了多少笔交易； 若小于limit，表示已经查询到最新的交易
    "details": [{ //  返回的details是一个数组，每个item是一笔交易
      "memo": "196702323",  // 此为解密过的memo，只有交易双方才能解密memo
      "description": "Transfer 3099 RUI from myuser to user1 -- Memo: 196702323   (Fee: 0.09493 RUI)", //交易描述
      "op": {  // 交易中包含的操作
        "id": "1.11.3436",
        "op": [0, {// 序号0表示转帐
          "fee": {
            "amount": 9493,// 操作的手续费为(9493/100000) RUI，数字要除以100000
            "asset_id": "1.3.1" //  资产1.3.1是RUI
          },
          "from": "1.2.3054",// 转帐的发起帐户
          "to": "1.2.2999",// 转帐的接收帐户
          "amount": {
            "amount": 300000,  // 转帐金额为(300000/100000) 即3个RUI ，数字要除以100000
            "asset_id": "1.3.1"
          },
          "memo": {
            "from": "RUI68o9LkFKv5ihSt6z9oTmc7wVALUmT5Kd75BTy9rMp38wSuWU5N",
            "to": "RUI5yRwAseZhPBorMmxXhQvmvsDzoJJbQnau5fFboGr5V2zGZPWsk",
            "nonce": "384187316390066",
            "message": "1c05a47a362361c0cf2dba52d08f3517"  // 此为加密的memo
          },
          "extensions": []
        }],
        "result": [0, {}],
        "block_num": 1115034, // 交易被打包进哪个区块，所属的区块号(区块高度)
        "trx_in_block": 0,// 交易在区块中的索引位置
        "op_in_trx": 0,
        "virtual_op": 3482
      },
      "transaction_id": "f9f8f8359c59ac1341516facdf30c98fd5d57b5b"
    }, {
      "memo": "",
      "description": "Transfer 7100 RUI from user1 to myuser   (Fee: 1 RUI)",
      "op": {
        "id": "1.11.3331",
        "op": [0, {
          "fee": {
            "amount": 100000,
            "asset_id": "1.3.0"
          },
          "from": "1.2.2999",
          "to": "1.2.3054",
          "amount": {
            "amount": 710000000,
            "asset_id": "1.3.1"
          },
          "extensions": []
        }],
        "result": [0, {}],
        "block_num": 1000766,
        "trx_in_block": 0,
        "op_in_trx": 0,
        "virtual_op": 3377
      },
      "transaction_id": "7c64c51ee931043ca1bc6791efc942e94b8236af"
    }]
  }
}
```

**注意：对交易所来说，转帐操作的资产id必须要是1.3.1\(RUI\)。**

#### 不可回退区块

调用cli_wallet的get_dynamic_global_properties接口，查看当前最大的不可回退区块号\(也即最大的不可回退区块高度\)。小于此区块高度的区块，其包含的交易都是已经被确认不可回退的。可用作用户提现时参考，区块不可回退时再处理用户提现。

```
curl --data '{"jsonrpc": "2.0", "method": "get_dynamic_global_properties", "params": [], "id": 1}' http://127.0.0.1:38091/rpc
```

返回结果：

```
{
  "id": 1,
  "result": {
    "id": "2.1.0",
    "head_block_number": 1724515, // 当前最大区块号(区块高度)
    "head_block_id": "001a506300f6717c3e99b6d8d89b264d94f1793c",
    "time": "2017-08-16T14:51:53",
    "current_witness": "1.6.2",
    "next_maintenance_time": "2017-08-16T15:00:00",
    "last_budget_time": "2017-08-16T14:50:00",
    "witness_budget": 0,
    "accounts_registered_this_interval": 0,
    "recently_missed_count": 1,
    "current_aslot": 2426557,
    "recent_slots_filled": "326968992855967788277935148493325729655",
    "dynamic_flags": 0,
    "last_irreversible_block_num": 1724507   // 最大的不可回退区块号(区块高度)为1724507
  }
}
```

## 六、注意事项

1. 用户充值。每笔转账可以带一个备注，交易所通过这个备注来区分是哪个用户的充值。具体备注与交易所用户关联关系，请交易所自行设定。备注是加密的，只有转帐双方才可以解密。
2. 转帐手续费问题。转帐的手续费由2部分组成：基本手续费 + memo费用，其中基本手续费为0.05RUI, memo费用按总字节长度收费，0.5RUI/`KB`。一次转帐手续费计算：`0.05 + 0.5*(KBs of memo) RUI ， 取KB时截断取整`。 带备注的转帐，手续费一般在\[0.05, 0.15\] RUI之间\(假设备注长度不超过100字节\)。
3. 交易所查询交易历史，处理用户充值时，需要处理用户充值时填入memo的特殊情况。比如交易所提供给用户的memo是1234567，而用户充值填写的memo是"我的memo是1234567!"，类似情况需要考虑。
4. 不建议交易所提供类似RUI123456的memo，把memo作为帐户进行充值。
   **memo长度和内容无限制，建议memo长度超过10位，并且以数字开头**。
5. 钱包的json rpc接口，帐户名需要传入小写，不能有大写字母。用户提现到RUI时，需要将绑定的帐户名转为小写。
6. **人工处理用户充值问题**：需要用户提供txID\(用户可以通过钱包查看当前转帐信息获得\)，txID可以保证用户充值转帐的唯一性。务必注意：**平台保存好人工处理过的txID, 同一txID只处理1次**。如果不同的注册用户使用同一txID提交工单的，属于欺骗！
7. 系统中存在多种资产，其中资产id（asset_id）1.3.1为RUI。监听用户充值时，请务必校验转帐交易中的asset_id字段为1.3.1。
8. 用户提现。调用transfer/transfer2处理用户提现时，转帐数量请传入字段串，加双引号。如下：
9. ```
   curl --data '{"jsonrpc": "2.0", "method": "transfer2", "params": ["from_account", "to_account", "100.01", "RUI", "",  true], "id": 1}' http://127.0.0.1:38091/rpc
   ```
10. RUI精度，为小数点后5位，即最小单位为0.00001 RUI。MyGrapheneChain中没有小数，数字在系统中被放大了10万倍，所以get_account_history / get_account_history_by_operations接口返回的数字，比如转帐的数量，需要除以10万，才是真正的数量。
11. 钱包状态为locked状态时只能查询，不能转帐，不能解密转帐备注。如果需要转帐或者查询交易历史，需要先unlock。
12. **transfer/transfer2转帐时，第3个参数转帐数量如果包含小数，必须加双引号，否则转帐会失败。建议转帐数量统一加上双引号。**
