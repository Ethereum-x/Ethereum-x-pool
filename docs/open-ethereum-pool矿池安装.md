# open-ethereum-pool矿池安装

首先安装getx节点，保证节点抵押10000ETX,并正常运行，请参照节点搭建教程。

```
//go >= 1.9
//geth or parity
//redis-server >= 2.8.0
//nodejs >= 4 LTS
//nginx

//安装go
wget https://dl.google.com/go/go1.9.4.linux-amd64.tar.gz
tar -zxvf go1.9.4.linux-amd64.tar.gz
mv go /usr/local/
mkdir -p /work/go
vim /etc/profile
	export GOPATH=/work/go
	export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
source /etc/profile
go version

//安装redis-server
apt-get -y install redis-server

//安装nodejs
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
apt-get install -y nodejs
npm config set registry https://registry.npm.taobao.org

//安装nginx
apt-get -y install nginx

//安装open-ethereum-pool
git config --global http.https://gopkg.in.followRedirects true
git clone https://github.com/Ethereum-x/Ethereum-x-pool.git
cd Ethereum-x-pool
make

//运行Ethereum-x-pool
cd /work/Ethereum-x-pool/
cp config.example.json config.json
./build/bin/open-ethereum-pool config.json

运行www
//修改www/config/environment.js
vim www/config/environment.js

ApiUrl: '//服务器地址/',

//编译www
cd www
npm install -g ember-cli@2.9.1
npm install -g bower
sudo npm install
bower install --allow-root
./build.sh

//配置nginx
vim /etc/nginx/sites-available/default

upstream api {
	server 127.0.0.1:8080;
}
root /work/Ethereum-x-pool/www/dist;
location /api {
	proxy_pass http://api;
}

//重启nginx
service nginx restart

//访问地址：http://服务器地址/
//API地址
http://服务器地址/api/stats
http://服务器地址/api/miners
http://服务器地址/api/blocks
http://服务器地址/api/payments
http://服务器地址/api/accounts/{login:0x[0-9a-fA-F]{40}}

```


