```
openssl genrsa -aes128 2048 > server.key
openssl rsa -in server.key  -out server.key.nopass

openssl req -new -key server.key.nopass > server.csr

openssl x509 -in server.csr -days 36500 -req -signkey server.key.nopass > server.crt


sudo cp server.crt /etc/httpd/ssl/crt 
sudo cp server.key.nopass /etc/httpd/ssl/key 



## host
csr, crt, key
cat server.csr
EDITOR=vim bundle exec knife solo data bag create certificates csr

knife data bag show certificates csr --secret-file ./encrypted_data_bag_secret --local


bundle exec knife solo prepare vagrant-moodle
bundle exec knife solo cook vagrant-moodle nodes/vagrant-moodle.json 

rm -f /var/lock/subsys/httpd


```
