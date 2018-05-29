# Macで環境構築


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
```
ssh-keygen -t rsa -b 4096 -C “you@armg.jp"
#Enter連打

ssh-add ~/.ssh/id_rsa

cat ~/.ssh/id_rsa.pub  | pbcopy #copyする

```

### ↓GitページにてSSHキーを設定する

https://github.com/settings/keys

右上の「New SSH key」を押して、貼って登録する


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

```


## VMを立ち上げる
```
mkdir path/to/workspace

cd path/to/workspace

git clone https://github.com/armg/mentough_kitchen.git

cd mentough_kitchent/vm/dev

vagrant up

```

