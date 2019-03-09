```
vagrant up
vagrant ssh
```

swith to root user
```
sudo su
```

yum update
```
vi /etc/yum.conf


//press a key switch insert mode, then goto 19th line
#releasever=latest
↓
releasever=latest

:wq

yum -y update
exit

```


postgresql96 install
```
sudo yum install -y postgresql96 postgresql96-server postgresql96-libs postgresql96-contrib

sudo service postgresql96 initdb
sudo service postgresql96 start
sudo chkconfig postgresql96 on

#confirm
sudo -u postgres -i psql -c 'SELECT version();'
```

eidt postgresql conf file
```
sudo vi /var/lib/pgsql96/data/postgresql.conf 
listen_addresses = 'localhost'
↓
listen_addresses = '*' 
port = 5432

:wq

sudo vi /var/lib/pgsql96/data/pg_hba.conf 
//modify
# IPv4 local connections:
host    all         all         127.0.0.1/32          trust
host    all         all         0.0.0.0/0             trust
:wq

sudo service postgresql96 restart
//change password to 'postgres'
sudo passwd postgres

```



ruby install
```
git --version
git clone https://github.com/sstephenson/rbenv.git ~/.rbenv

git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build

echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
source ~/.bash_profile

rbenv -v

rbenv install 2.4.4
rbenv global 2.4.4
ruby -v

gem install bundler
gem install rails
yum install sqlite-devel

```



```
//
sudo yum -y install postgresql96-devel
sudo yum -y install gcc72-c++.x86_64

gem install pg -- --with-pg-config=/usr/bin/pg_config

//
cd path/to/mentough_common/
cp config/settings.local.yml.sample config/settings.local.yml


cd path/to/MTOP2/
cp config/database.yml.example config/database.yml
cp config/settings.local.yml.sample config/settings.local.yml

bundle install

rails db:migrate

//
cd path/to/mentough/
cp config/database.yml.sample config/database.yml
cp config/settings.local.yml.sample config/settings.local.yml

bundle install


cd path/to/MTOP2
rails s -b 0.0.0.0

//open other bash window
cd path/to/mentough
rails s -b 0.0.0.0 -p 3001

```