# 账户注册
MyGChain采用账户模型，并且引入了推荐注册机制，因此在MyGChain上注册一个账号，需要以下三个要素:

* 推荐人，推荐人是链上已存在的账户，会使用你的账户名和公钥帮你注册一个账号
* 账户名，账户名在链上是唯一的，所以请记住在MyGChain上，<b>`账户名即地址`</b>。
* ECC公钥，以RUI开头，Base64 编码的ECC公钥。

有两种方式可以完成账户的注册:

## 1. 在线钱包
使用在线钱包在界面上完成注册步骤。注册时，页面会自动调用水龙头发起注册请求。

## 2. 手动注册
推荐对私钥安全要求较高的开发者使用这种方式完成注册，保证私钥是离线的。

### 步骤1: 通过cli_wallet来生成一对公私钥

```
cli_wallet --suggest-brain-key
{
  "brain_priv_key": "SHAP CASCADE AIRLIKE WRINKLE CUNETTE FROWNY MISREAD MOIST HANDSET COLOVE EMOTION UNSPAN SEAWARD HAGGIS TEENTY NARRAS",
  "wif_priv_key": "5J2FpCq3UmvcodkCCofXSNvHYTodufbPajwpoEFAh2TJf27EuL3",
  "pub_key": "RUI75UwALPEFECfHLjHyNSxCk1j7XzSvApQiXKEbanWgr7yvXXbdG"
}
```

字段解释

* brain_priv_key: 助记词，是私钥的原始文本，通过助记词可以还原出私钥
* wif_priv_key: 私钥，在程序中使用
* pub_key: 公钥，用于链上账户注册

### 步骤2: 通过水龙头来完成账户注册

注册命令为:

```
curl '<url>' -H 'Content-type: application/json' -H 'Accept: application/json’ -d ‘{“account”:{“name”:”<account_name>”,”owner_key”:”<public_key>”,”active_key”:”<public_key>”,”memo_key”:”<public_key>”,”refcode”:null,”referrer”:null}}’
```
account_name和public_key替换成对应的账号名称和公钥。私钥离线保存。
