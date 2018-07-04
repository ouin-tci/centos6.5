https://cross-black777.hatenablog.com/entry/2016/04/27/090000
```
yum update -y nss curl libcurl

openssl rand -base64 512 > encrypted_data_bag_secret

openssl genrsa -aes128 2048 > server.key
openssl rsa -in server.key  -out server.key.nopass

openssl req -new -key server.key.nopass > server.csr

openssl x509 -in server.csr -days 36500 -req -signkey server.key.nopass > server.crt



#sudo cp server.crt /etc/httpd/ssl/crt 
#sudo cp server.key.nopass /etc/httpd/ssl/key 

## host
csr, crt, key
cat server.csr

#linux?
export EDITOR=$(which vi)
bundle exec knife solo data bag create certificates csr
bundle exec knife data bag show certificates csr --secret-file ./encrypted_data_bag_secret --local

bundle exec knife solo data bag create certificates crt
bundle exec knife data bag show certificates crt --secret-file ./encrypted_data_bag_secret --local

bundle exec knife solo data bag create certificates key
bundle exec knife data bag show certificates key --secret-file ./encrypted_data_bag_secret --local

bundle exec knife solo prepare vagrant-moodle
bundle exec knife solo cook vagrant-moodle nodes/vagrant-moodle.json 

rm -f /var/lock/subsys/httpd

git.rb
+
package %w(nss curl libcurl) do
  action :upgrade
end

server.rb
mkdir /var/run/postgresql
chmod 777 /var/run/postgresql
ln -s /var/run/postgresql/.s.PGSQL.5432 /tmp/


#awscli にpython 2.7
sudo yum update # update yum
sudo yum install centos-release-scl # install SCL 
sudo yum install python27 # install Python 2.7
scl enable python27 bash
```



```
sudo yum -y update
sudo yum -y localinstall https://yum.postgresql.org/9.3/redhat/rhel-6.5-x86_64/pgdg-centos93-9.3-3.noarch.rpm
sudo yum -y install postgresql93-server
sudo service postgresql-9.3 initdb
sudo service postgresql-9.3 restart

sudo su
su - postgres
createuser -U postgres -sw moodle
createdb -U postgres -O moodle -E utf8 -T template0 moodle
echo "ALTER ROLE moodle ENCRYPTED PASSWORD 'moodle';" | psql -U postgres
exit
exit
```

```
vi /var/lib/pgsql/9.3/data/pg_hba.conf
以下内容を設定

# "local" is for Unix domain socket connections only
local   all             all                                     trust
# IPv4 local connections:
host    all             all             127.0.0.1/32            md5
# IPv6 local connections:
host    all             all             ::1/128                 md5

```

```
sudo rpm -ivh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
sudo yum install php php-cli php-common php-gd php-mbstring php-mysql php-pdo -y
sudo yum install php-pear php-devel postgresql93.x86_64 postgresql93-devel.x86_64 -y
sudo yum install php-pgsql.x86_64 -y

```
