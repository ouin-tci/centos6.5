# vagrant でcentos6.5にGit, Apache, PostgreSQL9.3, Ruby, Ruby on Railsをインストール
## chefを利用するとWindows ではChef Client インストールする必要がある。
### https://downloads.chef.io/chef#options
### 実際にどんなAPPをインストールしたのを把握するため、WindowsでのVM環境作成は手動で行う。


# HOST機(Windows)の操作

VirtualBoxをインストールする。

https://www.virtualbox.org/wiki/Downloads

利用OSに対応するパッケージをダウンロードしてインストールする

```
VirtualBox 5.2.12 platform packages
 Windows hosts
 OS X hosts
 Linux distributions
 Solaris hosts
``` 

Vagrantをインストールする

https://www.vagrantup.com/downloads.html

利用OSに対応するパッケージをダウンロードしてインストールする


mentough_kitchenをcheckoutする（Host機にgitのインストールが前提です）

https://github.com/armg/mentough_kitchen

あるいは

https://github.com/armg/mentough_kitchen/tree/release201806/vm/dev

VagrantFileを下記フォルダーに置く

***Windows7で当環境のvagrant起動だとPowerShell3が必要です。***

https://docs.microsoft.com/ja-jp/powershell/scripting/setup/installing-windows-powershell?view=powershell-6

```
mkdir path/to/workspace/vagrant
cd path/to/workspace/vagrant

vagrant up #VM起動
vagrant ssh #VMにログイン

sudo su #rootに切り替える、VMのため、セキュリティは考慮しない、全てrootで操作をする
source ~/.bash_profile #環境定数などを適用、rootに変わるたびに実行する必要があり
```

# 以下、Guest機(VM)の操作

# curlインストール

```
#バージョンアップしとく
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

#sshでgithubにログインできるよう

#デフォルトは[id_rsa]を作成する
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

#ログインするたびにssh-addを実行しないとだめ、めんどう
eval $(ssh-agent -s)
ssh-add ~/.ssh/id_rsa

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
#既存vagrant boxを利用するのが便利かも　https://app.vagrantup.com/fscm/boxes/postgresql

#Postgresql 10はこれ
#https://yum.postgresql.org/10/redhat/rhel-6.5-x86_64/pgdg-centos10-10-1.noarch.rpm

#取り合えず9.3をインストール
yum -y localinstall https://yum.postgresql.org/9.3/redhat/rhel-6.5-x86_64/pgdg-centos93-9.3-3.noarch.rpm
yum -y install postgresql93-server

#postgresql-#{version}で操作する

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

#外部からアクセスできるように、修正後DBを再起動して反映される
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

# PHP5.6インストール（Moodle1.9系をインストールする場合のみ）

```
#https://www.miraclelinux.com/tech-blog/bw58yw

rpm -Uvh http://ftp.iij.ad.jp/pub/linux/fedora/epel/6/x86_64/epel-release-6-8.noarch.rpm
rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm

yum install --enablerepo=remi --enablerepo=remi-php56 php php-opcache php-devel php-mbstring php-mcrypt php-mysqlnd php-phpunit-PHPUnit php-pecl-xdebug php-pecl-xhprof

```

# PHP7インストール（Moodle3.4系をインストールする場合のみ）

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

# Apache確認（Moodleをインストールする場合のみ）
```
#基本的にすでにOSにインストールしたはず
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
rbenv install 2.4.4
rbenv versions
rbenv global 2.4.4
```
```
# 依存ライブラリが古かったりなかったりすると失敗して
# 以下のようなログを吐くので、指示通りにすれば解決できるはず
The Ruby openssl extension was not compiled.
The Ruby readline extension was not compiled.
The Ruby zlib extension was not compiled.
ERROR: Ruby install aborted due to missing extensions
Try running `yum install -y openssl-devel readline-devel zlib-devel` to fetch missing dependencies.
```


# Ruby on Railsインストール
```
#http://railsdoc.com/setup
gem install bundler
gem install rails
```

# Rails bundle install 時にコンパイルエラー

```
#No pg_config... trying anyway. If building fails, please try again with
yum install postgresql93-devel
gem install pg -- --with-pg-config=/usr/pgsql-9.3/bin/pg_config

#sqlite3エラー
yum install sqlite-devel
```

# rails サーバー起動時エラー

```
there was an error while trying to load the gem 'uglifier'. (Bundler::GemRequireError)
# See https://github.com/rails/execjs#readme for more supported runtimes
  
gem 'mini_racer', platforms: :ruby
or 
gem 'therubyracer'
```

# sambaをインストール

https://daichan.club/linux/573

# phantomjs をインストール

```
#https://www.craneto.co.jp/archives/1203/#yum
$ wget https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-2.1.1-linux-x86_64.tar.bz2
$ tar jxvf phantomjs-2.1.1-linux-x86_64.tar.bz2
# お使いの環境により /usr/bin/ を /usr/local/bin/ に書き換えてください
$ cp phantomjs-2.1.1-linux-x86_64/bin/phantomjs /usr/bin/
$ phantomjs -v
# phantomjs -v でパーミッション・エラーが出る場合には適宜パーミッション変更のこと



1. 日本語を使うように環境設定する
sudo yum -y install fontconfig-devel
sudo yum groupinstall "Japanese Support"

sudo localedef -f UTF-8 -i ja_JP ja_JP.utf8

以下を~/.bash_profileに追記

LANG=ja_JP.UTF-8

export LANG

2. PhantomJSが日本語フォントとして認識できるフォントをインストールする
sudo yum -y install mkfontdir mkfontscale
wget "http://sourceforge.jp/frs/redir.php?m=osdn&f=%2Fmix-mplus-ipa%2F59021%2Fmigmix-medium-20130702.tar.xz"
tar Jxf redir.php\?m\=osdn\&f\=%2Fmix-mplus-ipa%2F59021%2Fmigmix-medium-20130702.tar.xz
cd migmix-medium-20130702/
mkdir -p /usr/share/fonts/ipa/TrueType
cp migmix-* /usr/share/fonts/ipa/TrueType/
mkfontdir
mkfontscale
fc-cache -f -v


```

#  sandboxからmoodleへアクセス
```
1. VgrantでMoodle VMを作成してsshでログインする
2. rbenvのインストール
3. Rubyのインストール

# HOST機にあるMoodle VM のprivate_keyをsandboxに持ち込み
#C:/workspace/mentough_kitchen/vm/moodle/.vagrant/machines/default/virtualbox/private_key

chmod 600 path/to/private_key


$ vi ~/.ssh/config
Host vagrant-moodle
  HostName XX.XX.XX.XX
  User vagrant
  Port 22
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile path/to/private_key
  IdentitiesOnly yes
  LogLevel FATAL

# knifeでMoodle環境作成
bundle exec knife solo prepare vagrant-moodle
```
