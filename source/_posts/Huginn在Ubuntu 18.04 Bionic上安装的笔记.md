---
title: Huginn在Ubuntu 18.04 Bionic上安装的笔记
date: 2019-09-23 23:59:24
categories: "Linux"
tags: ["Linux","VPS", "Ubuntu18"]
---

```bash
# 安装nodejs源 10.16.3 LTS
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -

apt-get install -y runit build-essential git zlib1g-dev libyaml-dev libssl-dev libgdbm-dev libreadline-dev libncurses5-dev libffi-dev curl openssh-server checkinstall libxml2-dev libxslt-dev libcurl4-openssl-dev libicu-dev logrotate python-docutils pkg-config cmake nodejs graphviz

# 编译安装ruby
mkdir /tmp/ruby && cd /tmp/ruby
curl -L --progress https://cache.ruby-lang.org/pub/ruby/2.6/ruby-2.6.4.tar.bz2 | tar xj
cd ruby-2.6.4/
./configure --disable-install-rdoc
make -j`nproc`
make install

# 安装下面的东西
gem install rake bundler foreman --no-document

# 升级rubygems
gem update --system --no-document

# 如果已经安装MySQL-server就不用安装了，只安装mysql-client libmysqlclient-dev
apt-get install -y mysql-server mysql-client libmysqlclient-dev

# 数据库操作
create user 'huginn'@'%' identified by 'password';
grant all privileges on `huginn_production`.* to 'huginn'@'%' identified by 'password' with grant option;
flush privileges;

# 创建用户huginn
useradd -U -m -s /usr/sbin/nologin -c huginn huginn

# 从源处克隆Huginn
cd /home/huginn
sudo -u huginn -H git clone https://github.com/cantino/huginn.git -b master huginn
cd /home/huginn/huginn
sudo -u huginn -H cp .env.example .env
sudo -u huginn mkdir -p log tmp/pids tmp/sockets
chmod o-r .env
sudo -u huginn -H cp config/unicorn.rb.example config/unicorn.rb
# vi .env
DATABASE_ADAPTER=mysql2
DATABASE_RECONNECT=true
DATABASE_NAME=huginn_production
DATABASE_POOL=20
DATABASE_USERNAME=huginn
DATABASE_PASSWORD='$password'

# 去掉注释
RAILS_ENV=production

# 安装Gems
sudo -u huginn -H bundle install --deployment --without development test

# 初始化数据库
sudo -u huginn -H bundle exec rake db:create RAILS_ENV=production
sudo -u huginn -H bundle exec rake db:migrate RAILS_ENV=production
sudo -u huginn -H bundle exec rake db:seed RAILS_ENV=production SEED_USERNAME=admin SEED_PASSWORD=password

# 生产安全Token，放到 .env 的APP_SECRET_TOKEN
bundle exec rake secret

# 编译前端资源文件
sudo -u huginn -H bundle exec rake assets:precompile RAILS_ENV=production

# 编辑配置
sudo -u huginn -H vi Procfile
# 注释
web: bundle exec rails server -p ${PORT-3000} -b ${IP-0.0.0.0}
jobs: bundle exec rails runner bin/threaded.rb
# 去掉注释
# web: bundle exec unicorn -c config/unicorn.rb
# jobs: bundle exec rails runner bin/threaded.rb

# 安装runit-systemd（Ubuntu 18.04 Bionic）
apt install -y runit-systemd
service start runit

#Export the init scripts:
sudo bundle exec rake production:export

# nginx的设置参照官方文档

# 问题记录

# 1、如果还是连接不上的话，可能是tmp/sockets目录被删除了,重新创建
sudo -u huginn mkdir -p log tmp/pids tmp/sockets
sudo bundle exec rake production:export

# 2、点击“Agents”页面中的“View diagram”会报错，去掉.env中的`USE_GRAPHVIZ_DOT`注释
```