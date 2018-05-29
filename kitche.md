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


## git用sshキーを作成する

```
ssh-keygen -t rsa -b 4096 -C “you@armg.jp"
Enter連打

ssh-add ~/.ssh/id_rsa

cat ~/.ssh/id_rsa.pub  | pbcopy #copyする

https://github.com/settings/keys

New SSH keyを押して、貼る

```


## VMを立ち上げる

```
mkdir path/to/workspace

cd path/to/workspace

git clone https://github.com/armg/mentough_kitchen.git

cd mentough_kitchent/vm/dev

vagrant up

```

