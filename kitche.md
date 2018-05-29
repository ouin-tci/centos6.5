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


vagrant ssh-config --host vagrant-mentough >> ~/.ssh/config #SSHでアクセスするための設定　設定せずにvagrant sshでもできそう

```



