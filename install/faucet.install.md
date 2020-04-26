# 水龙头安装

## 1. 安装依赖

```bash
apt -y install mysql-server libmysqlclient-dev libreadline-dev
```

数据库密码设置成123456

## 2. 安装ruby环境

```bash
cd ~
git clone git://github.com/sstephenson/rbenv.git .rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec $SHELL

git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
exec $SHELL

git clone https://github.com/sstephenson/rbenv-gem-rehash.git ~/.rbenv/plugins/rbenv-gem-rehash

rbenv install 2.2.3
rbenv global 2.2.3
gem install bundler
```

## 3. 下载水龙头代码

```bash
git clone https://github.com/my-graphene/faucet
cd faucet
bundle   # ignore warnings
```

## 4. 创建并初始化数据库

```bash
rake db:create; rake db:migrate; rake db:seed
RAILS_ENV=production bundle exec rake db:create db:schema:load
```

## 5. 运行

```bash
rails s -b 0.0.0.0
```
