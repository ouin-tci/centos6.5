# vagrant でcentos6.5にGit, Apache, PostgreSQL9.3, Ruby, Ruby on Railsをインストール

VagrantFileを下記フォルダーに置く

```
mkdir ~/workspace/vagrant
cd ~/workspace/vagrant

vagrant up
vagrant ssh

sudo su
source ~/.bash_profile
```

# curlインストール

```
#curl : (1) Protocol https not supported or disabled in libcurl
#https://stackoverflow.com/questions/19015282/how-do-i-enable-https-support-in-libcurl

yum install openssl-devel
wget https://curl.haxx.se/download/curl-7.54.0.tar.bz2
tar xf curl-7.54.0.tar.bz2
cd curl-7.54.0

cd /path/to/curl # step-1
./configure --enable-libcurl-option
make
make install
curl --version
```

# gitのインストール

```
yum install -y git

# git ssh public key追加
#https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/
#https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/

ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

#ログインするたびにssh-addを実行しないとだめ、めんどう
eval $(ssh-agent -s)
ssh-add -K ~/.ssh/id_rsa

#中身をコピーしてGit Accountに張る

cat ~/.ssh/id_rsa.pub

#github ssh key url
https://github.com/settings/keys
```

# PostgreSQL インストール

```
#https://yum.postgresql.org/9.3/redhat/rhel-6.5-x86_64/
#https://weblabo.oscasierra.net/postgresql-installing-postgresql9-centos6-1/
#https://www.postgresql.jp/document/9.3/html/index.html
#pgAdmin https://www.pgadmin.org/download/

yum -y localinstall https://yum.postgresql.org/9.3/redhat/rhel-6.5-x86_64/pgdg-centos93-9.3-3.noarch.rpm
yum -y install postgresql93-server

#DB初期化
service postgresql-9.3 initdb
#自動で起動する
chkconfig postgresql-9.3 on
#Start & Stop
service postgresql-9.3 start
service postgresql-9.3 stop

#IPとポートを設定する
vi /var/lib/pgsql/9.3/data/postgresql.conf 
listen_addresses = 'localhost'
↓
listen_addresses = '*' 
port = 5432

#外部からアクセスできるように
vi /var/lib/pgsql/9.3/data/pg_hba.conf 
# IPv4 local connections:
host    all         all         127.0.0.1/32          trust
host    all         all         0.0.0.0/0             trust


#postgres パスワードを変更

passwd postgres

#postgresでDB操作　
su - postgres

#基本操作
#http://www.marronkun.net/linux/other/database_000014.html
psql -l

```

# PHP5.6インストール

```
#https://www.miraclelinux.com/tech-blog/bw58yw

rpm -Uvh http://ftp.iij.ad.jp/pub/linux/fedora/epel/6/x86_64/epel-release-6-8.noarch.rpm
rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm

yum install --enablerepo=remi --enablerepo=remi-php56 php php-opcache php-devel php-mbstring php-mcrypt php-mysqlnd php-phpunit-PHPUnit php-pecl-xdebug php-pecl-xhprof

```

# PHP7インストール

```
#http://sonaiyuutemo.hatenablog.jp/entry/2017/05/19/172019
#epel.repoの確認
cd /etc/yum.repos.d
ll
#なかった場合は↓
yum install epel-release

#古いバージョンを削除
yum remove php-*
yum remove php* php-common

rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
yum -y install --enablerepo=remi,remi-php71 php php-devel php-mbstring php-mysql php-pdo php-gd

php --version


#phpinfoを確認
echo '<?php
  phpinfo();
?>' > /var/www/html/phpinfo.php

````

# Apache確認
```
#https://qiita.com/a-killer-bee/items/d679cb893f64ff18ad8f
#既にApacheがインスールされている場合はアンインストール

yum remove -y httpd
yum -y install httpd
httpd -v

#自動実行する
chkconfig httpd on

service httpd start

#IPで確認する
#http://xxx.xxx.xxx.xxx/
#phpinfoを確認する
http://xxx.xxx.xxx.xxx/phpinfo.php
```

# Moodleインストール

```
#http://shinetec.co.jp/e-learning/moodle/help.php?file=install.html
```

# herokuをインストール

```
#https://qiita.com/colorrabbit/items/ea5e1b144620ffc4acca
#https://devcenter.heroku.com/articles/heroku-cli#download-and-install

echo 'PATH=/usr/local/bin:$PATH' >> ~/.bash_profile 
source ~/.bash_profile

curl https://cli-assets.heroku.com/install-standalone.sh | sh
```


# rbenvのインストール

```
#centOS6.5のgit は1.7系、古いもの
#このエラーがでた時に error:  while accessing https://github.com/xxxx
#参考URL：https://qiita.com/murai-taisuke/items/173b3cbbd697e630047d
#yum update -y nss curl libcurl

git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile

#sudo suをしたときに要実行
source ~/.bash_profile
rbenv --version
```

# Rubyのインストール

```
rbenv install --list
yum install -y openssl-devel readline-devel zlib-devel
rbenv install 2.2.3
rbenv versions
```



# Ruby on Railsインストール
```
#http://railsdoc.com/setup

rbenv install --list 
rbenv install 2.4.1
rbenv versions 
rbenv global 2.4.1

gem install bundler
gem install rails
```

# Rails bundle install 時にpgコンパイルエラー

```
#postgresqlのバージョンにあわせる
#sudo yum install postgresql93-libs
#sudo yum install postgresql93-devel

#No pg_config... trying anyway. If building fails, please try again with
gem install pg -- --with-pg-config=/usr/pgsql-9.3/bin/pg_config

```
