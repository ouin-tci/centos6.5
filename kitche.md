macで環境構築


## git用sshキーを作成する

```
ssh-keygen -t rsa -b 4096 -C “you@armg.jp"
Enter連打

ssh-add ~/.ssh/id_rsa

cat ~/.ssh/id_rsa.pub  | pbcopy #copyする

https://github.com/settings/keys

New SSH keyを押して、貼る

```


mkdir path/to/workspace

cd path/to/workspace

git clone https://github.com/armg/mentough_kitchen.git

cd mentough_kitchent/vm/dev

vagrant up

