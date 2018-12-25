## Index Table
| belong | api name | description |
| --- | --- | --- |
| <graphenelib/action.h> | [current_receiver](#current_receiver) | 返回当前合约账号的id |
| <graphenelib/action.h> | [get_action_asset_id](#get_action_asset_id) | 返回本次调用向合约发送的资产id |
| <graphenelib/action.h> | [get_action_asset_amount](#get_action_asset_amount) | 返回本次调用向合约发送的资产数量 |
| <graphenelib/asset.h> | [withdraw_asset](#withdraw_asset) | 将当前合约的资产转移到外部账户 |
| <graphenelib/asset.h> | [get_balance](#get_balance) | 获取外部账户的某资产余额 |
| <graphenelib/crypto.h> | [sha256](#sha256) | 计算数据的sha256 |
| <graphenelib/crypto.h> | [sha512](#sha512) | 计算数据的sha512 |
| <graphenelib/crypto.h> | [ripemd160](#ripemd160) | 计算数据的ripemd160 |
| <graphenelib/crypto.h> | [verify_signature](#verify_signature) | 验证签名 |
| <graphenelib/global.h> | [get_head_block_num](#get_head_block_num) | 获取最新区块号 |
| <graphenelib/global.h> | [get_head_block_id](#get_head_block_id) | 获取最新区块hash |
| <graphenelib/global.h> | [get_head_block_time](#get_head_block_time) | 获取最新区块的时间，返回值单位秒 |
| <graphenelib/global.h> | [get_trx_sender](#get_trx_sender) | 获取调用合约的账号的instance_id |
| <graphenelib/global.h> | [get_account_id](#get_account_id) | 根据账号名获取账号的instance_id |
| <graphenelib/global.h> | [get_asset_id](#get_asset_id) | 根据资产名获取资产的instance_id |
| <graphenelib/system.h> | [graphene_assert](#graphene_assert) | 如果条件不满足，中断本次合约的执行并会滚所有状态 |
| <graphenelib/system.h> | [graphene_assert_message](#graphene_assert_message) | 如果条件不满足，输出必要的信息，但是本次合约的执行会继续 |
| <graphenelib/system.h> | [print](#print) | 用于调试时日志的打印 |


<a name="current_receiver"></a>
## uint64_t current_receiver()
返回当前合约的帐户id，取最后48位。 如果帐户id为1.2.12345，则后48位即12345

<a name="get_action_asset_id"></a>
## uint64_t get_action_asset_id();
调用合约时，向合约发送的资产id，取资产id的后48位。


<a name="get_action_asset_amount"></a>
## int64_t get_action_asset_amount();
调用合约时，向合约发送的资产数量（放大10万倍的）

<a name="withdraw_asset"></a>
## void withdraw_asset(uint64_t from, uint64_t to, uint64_t asset_id, int64_t amount)
将当前合约的资产转移到外部账户

\<uint64_t\> from: 从哪个账号转账，一般是_self <br>
\<uint64_t\> to: 转账到哪个外部账户，必须只传账号的instance_id，比如外部账户是1.2.33，那么传33即可 <br>
\<uint64_t\> asset_id: 指定转账的资产id，必须只传资产id的instance_id, 比如资产id是1.3.0， 那么传0即可 <br>
\<int64_t\> amount: 转账金额，这个数字包含了资产的精度，比如想转1个RUI，那么应该写100000 <br>




<a name="get_balance"></a>
## int64_t get_balance(int64_t account, int64_t asset_id)

获取外部账户的某资产余额

\<int64_t\> account: 外部账户的instace_id <br>
\<int64_t\> asset_id: 指定资产的instance_id

<a name="sha256"></a>
## void sha256(const char *data, uint32_t length, checksum256 *hash)
计算数据的sha256

\<char\> data: 用于计算sha256的字符串首地址 <br>
\<uint32_t\> length: data字符串的长度 <br>
\<const checksum256 *\> hash: 出参 用于存储计算的sha256


<a name="sha512"></a>
## void sha512(const char *data, uint32_t length, checksum512 *hash)

计算数据的sha512

\<char\> data: 用于计算sha512的字符串首地址 <br>
\<uint32_t\> length: data字符串的长度 <br>
\<const checksum512 *\> hash: 出参 用于存储计算的sha512



<a name="ripemd160"></a>
## void ripemd160(const char *data, uint32_t length, checksum160 *hash)

计算数据的ripemd160

\<char\> data: 用于计算ripemd160的字符串首地址 <br>
\<uint32_t\> length: data字符串的长度 <br>
\<const checksum160 *\> hash: 出参 用于存储计算的ripemd160 <br>



<a name="verify_signature"></a>
## bool verify_signature(const char *data, uint32_t datalen, const signature* sig,  const char *pub_key, uint32_t pub_keylen)

验证签名

\<const char\> data: 签名的原始字符串 <br>
\<uint32_t\> datalen: data字符串的长度 <br>
\<signature\> sig: 签名数据 <br>
\<const char *\> pub_key: 签名私钥对应的公钥 <br>
\<uint32_t\> pub_keylen: 公钥的长度



<a name="get_head_block_num"></a>
## int64_t get_head_block_num()

获取最新区块号



<a name="get_head_block_id"></a>
## int64_t get_head_block_id()

获取最新区块hash



<a name="get_head_block_time"></a>
## int64_t get_head_block_time()

获取最新区块的时间，返回值单位秒



<a name="get_trx_sender"></a>
## int64_t get_trx_sender()

获取调用合约的账号的instance_id



<a name="get_account_id"></a>
## int64_t get_account_id(const char * data, uint32_t length)

根据账号名获取账号的instance_id


\<const char *\> data: 账号名，例如nathan <br>
\<uint32_t\> length: 账号名的长度，例如nathan的长度是6



<a name="get_asset_id"></a>
## int64_t get_asset_id(const char * data, uint32_t length)

根据资产名获取资产的instance_id

\<const char *\> data: 资产名 <br>
\<uint32_t\> length: 账号名的长度，例如nathan的长度是6



<a name="graphene_assert"></a>
## void graphene_assert(uint32_t test, const char* msg)

如果条件不满足，中断本次合约的执行并会滚所有状态

\<uint32_t\> test:  <br>
\<const char*\> msg: <br>



<a name="graphene_assert_message"></a>
## void graphene_assert_message(uint32_t test, const char* msg, uint32_t msg_len)

如果条件不满足，输出必要的信息，但是本次合约的执行会继续

\<uint32_t\> test: <br>
\<const char*\> msg:  <br> 
\<uint32_t\> msg_len:  <br>



<a name="print"></a>
## inline void print( const char* ptr )；[example](examples/helloworld.md)

用于调试时日志的打印

\<const char*\> ptr: 
