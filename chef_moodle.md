https://cross-black777.hatenablog.com/entry/2016/04/27/090000
```
yum update -y nss curl libcurl

openssl rand -base64 512 > encrypted_data_bag_secret

openssl genrsa -aes128 2048 > server.key
openssl rsa -in server.key  -out server.key.nopass

openssl req -new -key server.key.nopass > server.csr

openssl x509 -in server.csr -days 36500 -req -signkey server.key.nopass > server.crt


sudo cp server.crt /etc/httpd/ssl/crt 
sudo cp server.key.nopass /etc/httpd/ssl/key 

## host
csr, crt, key
cat server.csr

#linux?
export EDITOR=$(which vi)
bundle exec knife solo data bag create certificates csr

knife data bag show certificates csr --secret-file ./encrypted_data_bag_secret --local


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

Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).

```

