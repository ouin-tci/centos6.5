# Macで環境構築
## 前提条件：管理者権限のユーザー

## VirtualBoxをインストールする。

https://www.virtualbox.org/wiki/Downloads

利用OSに対応するパッケージをダウンロードしてインストールする。

***インストールするときにセキュリティ許可する必要がある。***

***システム環境設定→セキュリティとプライバシー→左下の南京錠っぽいアイコンをクリックして設定を変更する。***

```
VirtualBox 5.2.12 platform packages
 Windows hosts
 OS X hosts
 Linux distributions
 Solaris hosts
``` 

## Vagrantをインストールする

https://www.vagrantup.com/downloads.html

利用OSに対応するパッケージをダウンロードしてインストールする


## brew install

***XCode command line toolも一緒にインストールされる***

↓ページの手順通りに

https://brew.sh/index_ja.html

```
brew --version  #確認する
```


## Gitをインストール
***git をダウンロードしてインストールする(セキュリティ警告が出た場合はControlキーを押してクリックする、インストーラを）***

https://git-scm.com/download/mac

***あるいはbrewよりインストール***

```
brew install git
```

## git用sshキーを作成する
> 手順： https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/

```
#Enterキー連打でも作成できるが、複数キーを作成する場合は、なんか入力しないとGithubに設定しても使えなそう、
#ハマった。注意してください。

ssh-keygen -t rsa -b 4096 -C "you@armg.jp"
Enter file in which to save the key (/Users/administrator/.ssh/id_rsa):  #デフォルトはid_rsa
Enter passphrase (empty for no passphrase): #なんか入力する、ssh-addするときに聞かれる
Enter same passphrase again: 

ssh-add -K ~/.ssh/id_rsa

pbcopy < ~/.ssh/id_rsa.pub #publicキーをcopyする

#Macでssh key Fingerprintを確認する方法
#ssh-keygen -E md5 -lf ~/.ssh/id_rsa.pub

```

### GithubページにてSSHキーを設定する

https://github.com/settings/keys

右上の「New SSH key」を押して、貼って登録する

> ssh public key が登録されたら git clone git@github.comのように操作できない事があり。
> いろいろ調べたが、解明できななかった。 git clone https://github.com/armg/tools.git でcloneできる。

```
git clone git@github.com:armg/tools.git
Cloning into 'tools'...
ssh_exchange_identification: read: Operation timed out
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

```

## Rubyをインストール
```
brew install rbenv ruby-build
rbenv --version

rbenv install -l
rbenv install 2.4.4 #必要なバージョンをインストール

echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile 
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
source ~/.bash_profile 

rbenv global 2.4.4

rbenv versions

ruby -v

gem install bunlder

```

```
#rbenv でrubyのバージョンを切り替えてできるが、うまく行かない時はrbenv versionsでrubyのバージョンを設定する箇所を表示する。
vi で編集すれば反映する。

rbenv versions
  system
  2.4.1
* 2.4.4 (set by /Users/user/.ruby-version)

```

## VMを立ち上げる

```
mkdir path/to/workspace

cd path/to/workspace

git clone https://github.com/armg/mentough_kitchen.git

cd path/to/mentough_kitchent/vm/dev

vagrant up #初回は200MB超えvagrant boxファイルをダウンロードするため、時間かかる。

#共有フォルダ/vagrantをNFSでmountするためにパスワードを要求される、管理者権限のユーザーじゃないとだめ。
#==> default: Preparing to edit /etc/exports. Administrator privileges will be required...
#Password:"

#ChefがSSHでアクセスするための設定
vagrant ssh-config --host vagrant-mentough >> ~/.ssh/config 

```

## 環境を作成する。

```
cd ../..
bundle install
bundle exec knife solo prepare vagrant-mentough  # VMゲスト側にChefをインストール。
bundle exec knife solo cook vagrant-mentough nodes/vagrant-mentough.json # プロビジョニングされる

```

## VMへログイン

```
cd path/to/mentough_kitchen/vm/dev
vagrant ssh

#色々確認します

service postgresql-10 status
ruby --version
rbenv --version
ls /vagrant

```
## VMの中にもGitのSSHキーを作成してGithubに登録する。

### Host機からコピーする方法
```
#Host
cp -r ~/.ssh path/to/mentough_kitchen/vm/dev/

#Guest(VM)
cp -r vagrant/.ssh ~/

```
### 新規に登録する方法
```
ssh-keygen -t rsa -b 4096 -C "you@armg.jp"
Enter file in which to save the key (/Users/administrator/.ssh/id_rsa):  #デフォルトはid_rsa
Enter passphrase (empty for no passphrase): #なんか入力する、ssh-addするときに聞かれる
Enter same passphrase again: 


#ログインするたびにssh-addを実行しないとだめ
eval $(ssh-agent -s) #こっちはLinuxで実行しないとだめ
ssh-add ~/.ssh/id_rsa

cat ~/.ssh/id_rsa.pub #publicキーをcopyする

#Bad ownerエラー
ssh -v -T git@github.com
OpenSSH_5.3p1, OpenSSL 1.0.1e-fips 11 Feb 2013
Bad owner or permissions on /home/vagrant/.ssh/config
--------------------
chmod 600 /home/vagrant/.ssh/config

```




## 各アプリをセットアップする
```
#herokuのエラーが発生するため、元手順の説明で、mflowファイルのL41をコメントアウトする。

#heroku plugins:install git@github.com:armg/heroku-config.git
#heroku-cli: Updating to 5.12.0-211263f... 21.5 MB/21.5 MB
#Installing plugin git@github.com:armg/heroku-config.git... !
# ▸    Error installing package. 
# ▸    npm ERR! Linux 2.6.32-431.el6.x86_64
#....

vi /vagrant/mflow/mflow

#L41
#cmd "heroku plugins:install #{app_to_armg('heroku-config')}"


```

```
git clone git@github.com:armg/tools.git --branch master /vagrant/mflow
cd /vagrant/mflow
./mflow init

#rbenv: version `2.4.1' is not installed (set by /vagrant/mflow/.ruby-version)
rbenv install 2.4.1

./mflow init

#postgresql Gemの依存ライブラリ
sudo yum -y install postgresql10-devel

cd /vagrant/mflow/MTOP2/
cp config/database.yml.example config/database.yml
cp config/settings.local.yml.sample config/settings.local.yml
#postgresql 10 のgemをインストールする
gem install pg -- --with-pg-config=/usr/pgsql-10/bin/pg_config
bundle install

cd /vagrant/mflow/mentough/
cp config/database.yml.example config/database.yml
cp config/settings.local.yml.sample config/settings.local.yml
#postgresql 10 のgemをインストールする
bundle install


cd /vagrant/mflow/mentough_common/
cp config/settings.local.yml.sample config/settings.local.yml

```

## PostgreSQL PW設定とDB作成

```
sudo passwd postgres
#postgresに設定する

su - postgres
createdb MTOP2 #DBを作成する
psql -l | grep MTOP2      #DBを確認する
exit
```


## Ruby on Railsをインストール

```
gem install rails

rails db:migrate 

```


## PostgreSQLを外部(PgAdmin)からアクセスできるように設定する（オプション）

```
#IPとポートを設定する
vi /var/lib/pgsql/10/data/postgresql.conf 
listen_addresses = 'localhost'
↓
listen_addresses = '*' 
port = 5432

vi /var/lib/pgsql/10/data/pg_hba.conf 
# IPv4 local connections:
host    all         all         127.0.0.1/32          md5
#↓変更する
host    all         all         0.0.0.0/0             trust

#修正後DBを再起動して反映される
service postgresql-10 restart
```




