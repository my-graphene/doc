# 链 api

## 1. api分类

RUI链api根据功能被分为如下几种类型：
| 类型 | 类型值 | 简介 |
| :--- | :--- | :--- |
| database | 0 | 数据库 |
| login | 1 | 获取链api |
| network_broadcast | 2 | 发起交易 |
| history | 3 | 账户交易历史 |
| ... | ... | ... |


### database api

| 命令 | 参数 | 返回值 | 说明 |
| :--- | :--- | :--- | :--- |
| [get_objects](witness_node/getobjects.md) | &lt;ids&gt; | 根据ID查询目标对象 |  |
| set_subscribe_callback | &lt;cb&gt; &lt;clear_filter&gt; | 注册全局订阅的回调 |  |
| set_data_transaction_subscribe_callback | &lt;cb&gt; &lt;clear_filter&gt; | 注册数据交易的回调 |  |
| unsubscribe_data_transaction_callback |  | 取消注册数据交易的回调 |  |
| set_data_transaction_products_subscribe_callback | &lt;cb&gt; &lt;ids&gt; | 注册特定数据产品ID的数据交易回调 |  |
| set_pending_transaction_callback | &lt;cb&gt; | 注册未确认的交易的回调 |  |
| set_block_applied_callback | &lt;cb&gt; | 注册区块是否被应用的回调 |  |
| cancel_all_subscriptions |  | 停止所有订阅（回调） |  |
| [get_block_header](witness_node/getblock-header.md) | &lt;block_num&gt; | 获取区块头信息 |  |
| get_transaction | &lt;block_num&gt; &lt;trx_in_block&gt; | 获得交易信息 |  |
| get_block | &lt;block_num&gt; | 获取区块信息 |  |
| get_recent_transaction_by_id | &lt;id&gt; | 根据TXID查询交易，若交易超出有效期则会返回空值 |  |
| get_chain_properties |  | 获取链属性 |  |
| get_global_properties |  | 获取全局属性 |  |
| get_commission_percent |  | 获取佣金比例 |  |
| get_config |  | 获取编译时常量 |  |
| get_chain_id |  | 获取链ID |  |
| get_dynamic_global_properties |  | 获取动态全局属性 |  |
| get_key_references | &lt;key&gt; | 返回所有指向公钥的帐户信息 |  |
| is_public_key_registered | &lt;public_key&gt; | 验证公钥是否已经被注册 |  |
| get_accounts | &lt;account_ids&gt; | 通过ID获取账户信息 |  |
| [get_full_accounts](witness_node/getfullaccounts.md) | &lt;names_or_ids&gt; &lt;subscribe&gt; | 获取符合条件的所有账户相关信息 |  |
| get_account_by_name | &lt;name&gt; | 通过账户名获取账户信息 |  |
| get_account_references | &lt;account_id&gt; | 获取账户account_id相关的账户id |  |
| lookup_account_names | &lt;account_names&gt; | 通过账户名获取账户信息 |  |
| [lookup_accounts](witness_node/lookupaccounts.md) | &lt;limit&gt; &lt;lower_bound_name&gt; | 获取已注册账户的账户名和ID |  |
| get_account_count |  | 获取链上注册的所有账户数量 |  |
| [get_account_balances](witness_node/getaccount-balances.md) | &lt;id&gt; &lt;assets&gt; | 通过账户ID和资产ID获取账户资产信息 |  |
| get_named_account_balances | &lt;name&gt; &lt;assets&gt; | 通过账户名和资产ID获取账户资产信息 |  |
| get_balance_objects | &lt;&lt;\[address\]&gt;&gt; | 返回地址address上所有未领取的余额对象 |  |
| get_vested_balances | &lt;objs&gt; | 通过账户余额ID获取可领取的资产信息 |  |
| get_vesting_balances | &lt;account_id&gt; | 通过账户ID获取归属该账户但暂时不可领取的余额信息 |  |
| list_data_transaction_complain_requesters | &lt;start_date_time&gt; &lt;end_date_time&gt; &lt;limit&gt; | 通过开始和结束时间获取投诉的发起人，并返回前limit个 |  |
| list_data_transaction_complain_datasources | &lt;start_date_time&gt; &lt;end_date_time&gt; &lt;limit&gt; | 通过开始和结束时间获取被投诉的数据源，并返回前limit个 |  |
| get_assets | &lt;asset_ids&gt; | 通过资产ID获取资产 |  |
| [list_assets](witness_node/listassets.md) | &lt;lower_bound_symbol&gt; &lt;limit&gt; | 通过资产符号名称获取资产信息，并返回前limit个 |  |
| lookup_asset_symbols | &lt;symbols_or_ids&gt; | 通过资产符号获取资产列表 |  |
| get_witnesses | &lt;witness_ids&gt; | 通过见证人ID获取见证人列表 |  |
| get_witness_by_account | &lt;account&gt; | 通过账户ID获取见证人信息 |  |
| lookup_witness_accounts | &lt;lower_bound_name&gt; &lt;limit&gt; | 获取已注册见证人的ID和账户名 |  |
| get_witness_count |  | 获取已注册见证人的数量 |  |
| get_committee_members | &lt;committee_member_ids&gt; | 通过ID获取理事会成员信息 |  |
| get_committee_member_by_account | &lt;account&gt; | 通过账户ID获取理事会成员信息 |  |
| lookup_committee_member_accounts | &lt;account&gt; &lt;limit&gt; | 获得已注册理事会成员的ID和账户名,并返回前limit个 |  |
| get_workers_by_account | &lt;account&gt; | 通过账户ID获取工作对象信息 |  |
| lookup_vote_ids | &lt;votes&gt; | 通过投票对象ID来获得投票对象 |  |
| get_transaction_hex | &lt;trx&gt; | 获取签名的交易信息的十六进制编码 |  |
| get_required_signatures | &lt;trx&gt; &lt;available_keys&gt; | 获取签名的交易信息的签名公钥 |  |
| get_potential_signatures | &lt;trx&gt; | 获取签名的交易信息的签名公钥 |  |
| get_potential_address_signatures | &lt;trx&gt; | 获取签名的交易信息的地址 |  |
| verify_authority | &lt;trx&gt; | 验证交易是否已满足全部签名要求 |  |
| verify_account_authority | &lt;name_or_id&gt; &lt;signers&gt; | 验证签名人是否有足够的权力控制一个帐户 |  |
| validate_transaction | &lt;trx&gt; | 在当前情况下验证交易而不广播交易 |  |
| get_required_fees | &lt;ops&gt; &lt;id&gt; | 通过操作ID和资产ID获取手续费 |  |
| get_proposed_transactions | &lt;id&gt; | 通过具体账户ID获得相关的被提议的交易 |  |
| get_blinded_balances | &lt;id&gt; | 通过委托ID获取隐藏资产 |  |
| get_data_transaction_product_costs | &lt;start&gt; &lt;end&gt; | 获取指定时间内数据交易的产品费用 |  |
| get_data_transaction_total_count | &lt;start&gt; &lt;end&gt; | 获取指定时间内数据交易的次数 |  |
| get_merchants_total_count |  | 获取当前商户个数 |  |
| get_data_transaction_commission | &lt;start&gt; &lt;end&gt; | 获取指定时间内数据交易的佣金 |  |
| get_data_transaction_pay_fee | &lt;start&gt; &lt;end&gt; | 获取指定时间内数据交易的手续费 |  |
| get_data_transaction_product_costs_by_requester | &lt;requester&gt; &lt;start&gt; &lt;end&gt; | 获取请求账户（即商户）在指定时间内数据交易产生的产品费用 |  |
| get_data_transaction_total_count_by_requester | &lt;requester&gt; &lt;start&gt; &lt;end&gt; | 获取请求账户（即商户）在指定时间内发起数据交易的次数 |  |
| get_data_transaction_pay_fees_by_requester | &lt;requester&gt; &lt;start&gt; &lt;end&gt; | 获取请求账户（即商户）在指定时间内发起数据交易的手续费 |  |
| get_data_transaction_product_costs_by_product_id | &lt;product_id&gt; &lt;start&gt; &lt;end&gt; | 获取在指定时间内购买指定产品的产品费用 |  |
| get_data_transaction_total_count_by_product_id | &lt;product_id&gt; &lt;start&gt; &lt;end&gt; | 获取在指定时间内购买指定产品的次数 |  |
|  |  |  |  |

### history api

| 命令 | 参数 | 说明 | 备注 |
| :--- | :--- | :--- | :--- |
| get_account_history | &lt;account_id&gt; &lt;start&gt; &lt;limit&gt; &lt;stop&gt; | 查询帐户的交易历史，其中start/stop为operation_history_id， id为1.11.x |  |
| get_account_history_by_operations | &lt;account_id&gt; &lt;operation_ids&gt; &lt;start&gt; &lt;limit&gt; | 查询帐户的交易历史，根据operations_ids筛选指定类型的交易历史，其中start为序号，从1开始 |  |



## 2. 访问方式

进行api调用前，请详读[api rpc格式](format.md)

链api的访问有两种方式，即 `http+rpc` 和 `websocket+rpc` 方式。

witness_node服务接口为 `<ip>:<port1>` 。






### http+rpc

进入命令行，通过curl进行post请求调用

```
curl http://<ip>:<port1>/rpc -H "Content-Type:application/json" -X POST -d '{"id":1,"method":"call","params":[api类型值,"api指令",[参数]]}'
或
curl http://<ip>:<port1>/rpc -H "Content-Type:application/json" -X POST -d '{"id":1,"method":"call","params":["api类型串","api指令",[参数]]}'
```
即可看到返回结果。


<b>具体请求URL为</b>

`http://<ip>:<port1>/rpc`


eg. 

```
获取所有0(database) api接口
curl http://<ip>:<port1>/rpc -H "Content-Type:application/json" -X POST -d '{"id":1,"method":"call","params":[0,"",[]]}'
类型字串方式也可
curl http://<ip>:<port1>/rpc -H "Content-Type:application/json" -X POST -d '{"id":1,"method":"call","params":["database","",[]]}'

获取database中的1.2.17和1.2.18账户的信息
curl http://<ip>:<port1>/rpc -H "Content-Type:application/json" -X POST -d '{"id":1,"method":"call","params":[0,"get_accounts",[["1.2.17","1.2.18"]]]}'

获取history中的1.2.26账户的10条交易历史
curl http://<ip>:<port1>/rpc -H "Content-Type:application/json" -X POST -d '{"id":1,"method":"call","params":[3,"get_account_history",["1.2.26","1.11.0",10,"1.11.0"]]}'
```


<b>对于database api，其api类型值为0。因此还可以如下调用：</b>
```
获取database中的1.2.*几个账户的信息
curl http://<ip>:<port1>/rpc -H "Content-Type:application/json" -X POST -d '{"id":1,"method":"get_accounts","params": [["1.2.0","1.2.1","1.2.3","1.2.4","1.2.5","1.2.6","1.2.7","1.2.8","1.2.9","1.2.10","1.2.11","1.2.12","1.2.13","1.2.14","1.2.15","1.2.16","1.2.17","1.2.18","1.2.19"]]}'

获取链上资产信息
curl http://<ip>:<port1>/rpc -H "Content-Type:application/json" -X POST -d '{"id":1,"method":"get_assets","params":[["1.3.0","1.3.1"]]}'
```







### websocket+rpc

进入命令行，通过wscat进行请求调用，wscat安装方式`apt -y install node-ws`。

```
wscat -c ws://<ip>:<port1>
```

<b>具体请求URL为</b>

`ws://<ip>:<port1>`


eg.

```
获取所有0(database) api接口
{"id":1,"method":"call","params":[0,"",[]]}

字符串方式也可
{"id":1,"method":"call","params":["database","",[]]}

获取database中的1.2.17和1.2.18账户的信息
{"id":1,"method":"call","params":[0,"get_accounts",[["1.2.17","1.2.18"]]]}

获取history中的1.2.26账户的10条交易历史
{"id":1,"method":"call","params":[3,"get_account_history",["1.2.26","1.11.0",10,"1.11.0"]]}
```

<b>对于database api，其api类型值为0。因此还可以如下调用：</b>
```
获取database中的1.2.*几个账户的信息
{"id":1,"method":"get_accounts","params": [["1.2.0","1.2.1","1.2.3","1.2.4","1.2.5","1.2.6","1.2.7","1.2.8","1.2.9","1.2.10","1.2.11","1.2.12","1.2.13","1.2.14","1.2.15","1.2.16","1.2.17","1.2.18","1.2.19"]]}

获取链上资产信息
{"id":1,"method":"get_assets","params":[["1.3.0","1.3.1"]]}
```

