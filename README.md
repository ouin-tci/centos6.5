# centos6.5
mkdir ~/workspace/vagrant
cd ~/workspace/vagrant

vagrant init
vagrant box add chef/centos-6.6

$ 1) virtualbox
$ 3) vmware_desktop

$ Enter your choice: 1


vagrant up
vagrant ssh

sudo su
source ~/.bash_profile



# curlのopenssl更新
`
curl : (1) Protocol https not supported or disabled in libcurl
`
[https://stackoverflow.com/questions/19015282/how-do-i-enable-https-support-in-libcurl]

`
#curlオプション
curl version:     7.54.0
Host setup:       x86_64-pc-linux-gnu
Install prefix:   /usr/local
Compiler:         gcc
SSL support:      enabled (OpenSSL)
SSH support:      no      (--with-libssh2)
zlib support:     enabled
GSS-API support:  no      (--with-gssapi)
TLS-SRP support:  no      (--enable-tls-srp)
resolver:         default (--enable-ares / --enable-threaded-resolver)
IPv6 support:     enabled
Unix sockets support: enabled
IDN support:      no      (--with-{libidn2,winidn})
Build libcurl:    Shared=yes, Static=yes
Built-in manual:  enabled
--libcurl option: enabled (--disable-libcurl-option)
Verbose errors:   enabled (--disable-verbose)
SSPI support:     no      (--enable-sspi)
ca cert bundle:   /etc/pki/tls/certs/ca-bundle.crt
ca cert path:     no
ca fallback:      no
LDAP support:     no      (--enable-ldap / --with-ldap-lib / --with-lber-lib)
LDAPS support:    no      (--enable-ldaps)
RTSP support:     enabled
RTMP support:     no      (--with-librtmp)
metalink support: no      (--with-libmetalink)
PSL support:      no      (libpsl not found)
HTTP2 support:    disabled (--with-nghttp2)
Protocols:        DICT FILE FTP FTPS GOPHER HTTP HTTPS IMAP IMAPS POP3 POP3S RTSP SMB SMBS SMTP SMTPS TELNET TFTP
`
cd /path/to/curl # step-1
./configure --with-ssl # step-2
make # step-3
make install # step-4 (if not root, use sudo before command)

#gitのインストール
yum install -y git

#git ssh public key追加
# https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/
# https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/

ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

#ログインするたびにssh-addを実行しないとだめ、めんどう
eval $(ssh-agent -s)
ssh-add -K ~/.ssh/id_rsa


#中身をコピーしてGit Accountに張る

cat ~/.ssh/id_rsa.pub

#github ssh key url
https://github.com/settings/keys



#rbenvのインストール
#gitがこのエラーがでた時に error:  while accessing https://github.com/xxxx
#参考URL：https://qiita.com/murai-taisuke/items/173b3cbbd697e630047d
#yum update -y nss curl libcurl

git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile

#rootにsuしたときに要実行
source ~/.bash_profile
rbenv --version

#Rubyのインストール
rbenv install --list
yum install -y openssl-devel readline-devel zlib-devel
rbenv install 2.2.3
rbenv versions


#PostgreSQL インストール
#https://yum.postgresql.org/9.3/redhat/rhel-6.5-x86_64/
#https://weblabo.oscasierra.net/postgresql-installing-postgresql9-centos6-1/

yum -y localinstall https://yum.postgresql.org/9.3/redhat/rhel-6.5-x86_64/pgdg-centos93-9.3-3.noarch.rpm
yum -y install postgresql93-server

#DB初期化
service postgresql-9.3 initdb
#自動で起動する
chkconfig postgresql-9.3 on
#Start & Stop
service postgresql-9.3 start
service postgresql-9.3 stop


#PHP5.6インストール
#https://www.miraclelinux.com/tech-blog/bw58yw

rpm -Uvh http://ftp.iij.ad.jp/pub/linux/fedora/epel/6/x86_64/epel-release-6-8.noarch.rpm
rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm

yum install --enablerepo=remi --enablerepo=remi-php56 php php-opcache php-devel php-mbstring php-mcrypt php-mysqlnd php-phpunit-PHPUnit php-pecl-xdebug php-pecl-xhprof

#-------------------------------------------
#PHP7インストール
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
#s------------------------------------------------

php --version

#phpinfoを確認
echo '<?php
  phpinfo();
?>' > /var/www/html/phpinfo.php

#Apache確認
#https://qiita.com/a-killer-bee/items/d679cb893f64ff18ad8f
httpd -v
chkconfig httpd on

service httpd start
#http://ip/phpinfo.phpで確認


#Moodleインストール
http://shinetec.co.jp/e-learning/moodle/help.php?file=install.html


#herokuをインストール
#https://qiita.com/colorrabbit/items/ea5e1b144620ffc4acca
#https://devcenter.heroku.com/articles/heroku-cli#download-and-install

curl https://cli-assets.heroku.com/install-standalone.sh | sh
echo 'PATH="/usr/local/lib/heroku/bin/:$PATH"' >> ~/.bash_profile
