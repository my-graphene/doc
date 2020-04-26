
# ui介绍

[TOC]

MyGChain数据交易客户端ui是基于Nodejs开发的一个部署在商户和数据源本地的客户端，商户和数据源可以通过本地调用的方式购买和出售数据，数据交易的全程请求参数和回传数据都是经过加密处理的，而ui简化了这样的一个流程。

ui基于Nodejs开发，执行环境为Ubuntu16.04。ui需要安装Nodejs8.0以上版本.

## 1. 安装nodejs

```bash
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
apt install -y nodejs
node -v
npm -v
```

## 2. 配置npm源

```bash
vim ~/.npmrc

registry=https://registry.npm.taobao.org
sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
phantomjs_cdnurl=http://npm.taobao.org/mirrors/phantomjs
ELECTRON_MIRROR=http://npm.taobao.org/mirrors/electron/
```

## 3. 安装yarn

```bash
npm i -g yarn
```

## 4. 下载及安装依赖包

``` bash
git clone https://github.com/my-graphene/ui
cd ui
yarn
```

## 5. 修改服务节点

```bash
vim ui/app/api/apiConfig.js

{url: ws://ip:port", location: "Locally hosted"}
DEFAULT_FAUCET: "http://ip:port",
```

修改这两行中的ip和port为私链地址和端口即可

## 6. 开发模式启动

```bash
npm start
```
