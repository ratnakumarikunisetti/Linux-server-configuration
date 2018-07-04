Udacity-Linux-Server-Configuration
About
This is the Udacity project 6 about the Configuring the Linux the server.

Server Details
Server IP Address 13.126.48.184

Hosted site Url http://13.126.48.184.xip.io/

How to connect as grader:
save private key provided in your local machine and run the following command

ssh -i path/to/privatekey -p 2200 grader@13.126.48.184
  
Configuring Linux Server
Updating all packages
sudo apt-get update
sudo apt-get upgrade
Creating grader User:
sudo adduser grader
This will add new user

sudo nano /etc/sudoers
Below the Root user append the following line

grader  ALL=(ALL:ALL) ALL
This will grant sudo permission to grader

Creating a ssh key pair for grader
On your local machine in terminal/command prompt

ssh-keygen
This will generate public and private ssh keys which is saved to .ssh folder
id_rsa:
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAtVvCMEAmUwMWLZzJ0PLk0jMtrxPxNG3Ow4bKDgjXiUqIIx6g
76UJPdltDOIvumCNcLdvMiaFGlhU2V6CJOwybsqu6NfrtjcHs5FvvldOPzmqKgtZ
Nk5ld7Vq0VqfWR6q/vwjYHLKLAH9hufQbRNhbluB+0KI6qxDfcaeGehmzVh0hfB2
Wm8fAkZV/sIq3QkhGqvi1Lm2zAuoeBWiziIT9o2yrQQA8S7je0kaXpprnCYBBWxl
9GMVcDynbZ5I/bcTo1EitNF/LfkiRhqN/dDVWiFV35zwcN0LjJpLZrVi8R8uVcyK
eIdXQ+r65gUKnLoK1pHFa7JpTUfUgz0lzXfXnwIDAQABAoIBADH0aHTp9qR/ASjn
Ox/3B9huiHLlR1dtt7pb8mQTQ2tpwD4MPcBE8Vq7/THDS+pGli8qF9L0kU1Eb3rA
cZDCDtI9uhghAahbWB+6O9FuMvtvYtPZ9GTlC0YCDr5D/AiMTgWWZhg3BkFA+xih
2eNxpzDCu/b9yMD7WkvW3c29GjQNZoC5TyYNGyfGk4qv4m9PiVNxc2Nr3cnRyGZc
GaDcKnqL2zJH0EirPgy05TlnfdAn2TFFCaZ/13PwNZ8M2NzP9wEJA0O+DCxuvje2
iHOFfBCN8dVPzRTi7y8X0T60Hhg9Qo9qvTMMtaeWF40ViL3EieN7vVT8NjU7FWzr
376WAwECgYEA5MKHeB3mXQAOONKTZhkk/0uGN6ZYYQtF2nDEMD/9BJZ/1TfZHAHS
CbjflADB3H1FlRUC1kmT+J7AtvYhctxglHo9Fca2jLjaJUx9HitQpYlKvSLGlU45
W079dbgJCpBwnM0C/zy3quA4IXAmop9CPY4Nbf0Q3H8yWGk71J567z8CgYEAyvRA
xuHFRC4ZjIGBWCXw1FZXF72QkduNjw/R34KpLpecj3y53JbvY7cjjjgADP+Y/Fhb
kMBmbV72C9GWtExv6YF0piI3iaa2zbAzwXonBVtGibdP07kS7NnY9VsZE2UdmKwN
Q8qeJHjcU+J7VqRkPJPTHvsA6o0PCYRYm4PhX6ECgYA3+lY2EXL+jPXt97F0CXEh
O7TzRzRXQu/r/S409GOQzNcpMMpi7Rsdn+yuBeVqdAkj6wlPsJ+R9h8IZoBW4BCO
JL9v5blkBBP1jpsLV+QbLdZpI+pePM8SRekF9mvX9vJnnE9Ab/YtzUJPBGef4cLO
10T4BjYrDsEeG1o1tDY29wKBgDTomh5+6wtMLVLozAxrz627WHcS7yZnIy9Bg4gO
Kwa/dYweiuGL45qOOtGvnavF0l8utag10D4A/Im2OOCF8MLiAcPxtaLH+G4E2mk8
7AFEe04ZoNDkNZ/TZvEHr7DTsnSDne2nW7TMYTvpFhhPQOZd7zLrYqDC50Gm4+ae
6dkhAoGBAKGm0L9rsXJvcQh/L3b7e5xPEeOOroXVUbDioBZdnulusHPPU7MLHLDs
wc7w2ISim3pqJwO/GsBz/F0aVuTtea9Rw3Rb/r+gu+0FOcyIj+x6PVXfZstUxxBz
QqzWFPUXLVnWPl//90yroyDdxrUKIXHE1VZakbqTKYf4IHMnd3as
-----END RSA PRIVATE KEY-----
id_rsa pub:
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC1W8IwQCZTAxYtnMnQ8uTSMy2vE/E0bc7DhsoOCNeJSogjHqDvpQk92W0M4i+6YI1wt28yJoUaWFTZXoIk7DJuyq7o1+u2NwezkW++V04/OaoqC1k2TmV3tWrRWp9ZHqr+/CNgcsosAf2G59BtE2FuW4H7QojqrEN9xp4Z6GbNWHSF8HZabx8CRlX+wirdCSEaq+LUubbMC6h4FaLOIhP2jbKtBADxLuN7SRpemmucJgEFbGX0YxVwPKdtnkj9txOjUSK00X8t+SJGGo390NVaIVXfnPBw3QuMmktmtWLxHy5VzIp4h1dD6vrmBQqcugrWkcVrsmlNR9SDPSXNd9ef hp@DESKTOP-E82NM60
known_hosts
13.232.84.38 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBAOG775OzxLrhsqKfsbIfJ/nBr8pUCyrwi99/if0fyNqvTuC8u8I7SWcygh3/EXM/1W3RvSQe7J49r0/KEzN91I=
13.232.29.129 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBBx0u1krBWwQcf4v5YcLPq3Rd1UxPs2KyJNzK5FQzX8GFgku9nUq+hXydO/RoBu8oHzIxqFBKPmABAnQggxmfqA=
13.232.43.94 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBLeEktMk2paNNKk9+CIOQ6y7zdqYetkW70v56mvUFvLdhMWx27MRCw9R8YOProOB64C8LwR228z4T5KtwYG/OQM=
13.126.48.184 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBAHsWrX3KV+ODfnXh2UtP4QuQuobIjaI3iBeKBRLlgcYXf6NGbcaeF5Va/8n9ASbEapWRatUcCSu1O4GMV51QvQ=





Then in your virtual machine change to newly created user


su - grader
Create a new directory .ssh and new file authorized_keys in that directory

mkdir .ssh
sudo nano .ssh/authorized_keys
Copy the public key with .pub extension to authorized_keys and save the file

chmod 700 .ssh
chmod 644 .ssh/authorized_keys
700 will give read write and execute permission to user.
644 prevent other user from writting in to file. Then restart ssh server
service ssh restart
Now from your log in to grader with private key generated

ssh -i .ssh/id_rsa grader@13.126.48.184
Changing the ssh port to 2200
sudo nano /etc/ssh/sshd_config
Change port 22 to port 2200

Restart the ssh server

service ssh restart
Note: Before Logging using ssh add custom TCP port 2200 under lightsaail firewall in networking tab in lightsail instance console

Now Login using command like this

ssh -i .ssh/id_rsa -p 2200 grader@ipaddress
Disabling ssh login as root
sudo nano /etc/ssh/sshd_config

make change PermitRootLogin no

Configurating Ufw firewall
sudo ufw allow 2200/tcp
sudo ufw allow 80/tcp
sudo ufw allow 123/udp
sudo ufw enable
This will allow all required ports and enables the ufw

After that

sudo ufw status
It will display all allowed ports


select none from list and then select utc.

Installing Apache2
In terminal

sudo apt-get install apache2

Now mod_wsgi

sudo apt-get install python-setuptools libapache2-mod-wsgi

Enable mod_wsgi

sudo a2enmod wsgi

Setting up your flask application to work with apache2
Creating a flask app

In /var/www directory create a new folder sudo mkdir FlaskApp

Install git

sudo apt-get install git

move to the FlaskApp cd FlaskApp

In that direcory clone your github repository

sudo git clone https://github.com/ratnakumarikunisetti/buid-itemcatalog1.git


Rename your repository to FlaskApp

Then rename your project file to __init__.py

Error : While accesssing the client_secrets.json file

PROJECT_ROOT = os.path.realpath(os.path.dirname(__file__))
json_url = os.path.join(PROJECT_ROOT, 'client_secrets.json')
CLIENT_ID = json.load(open(json_url))['web']['client_id']
Use json_url instead client_secrets.json in script

Reffered from stack overflow

Install and configuring postgresql for project
Install Postgres sudo apt-get install postgresql

login to postgres sudo su - postgres

postgres shell psql

create user CREATE USER catalog WITH PASSWORD 'password';

permit user to createdb ALTER USER catalog CREATEDB;

Create a db name catalog with user catalog CREATE DATABASE catalog WITH OWNER catalog;

connect to db \c catalog

revoke all permission to public REVOKE ALL ON SCHEMA public FROM public;

Give schema permission to user catalog GRANT ALL ON SCHEMA public TO catalog;

exit from db and postgres \q and exit

Change the database connection in both db_setup.py and __init__.py as engine = create_engine('postgresql://catalog:password@localhost/catalog')

Now you are ready with your applicatiom

Configure and Enable a New Virtual Host
sudo nano /etc/apache2/sites-available/FlaskApp.conf

In this add the following code

<VirtualHost *:80>
 	ServerName 13.126.48.184
 	ServerAdmin admin@13.126.48.184
 	WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi
 	<Directory /var/www/FlaskApp/FlaskApp/>
 		Order allow,deny
 		Allow from all
 	</Directory>
 	Alias /static /var/www/FlaskApp/FlaskApp/static
 	<Directory /var/www/FlaskApp/FlaskApp/static/>
 		Order allow,deny
 		Allow from all
 	</Directory>
 	ErrorLog ${APACHE_LOG_DIR}/error.log
 	LogLevel warn
 	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
Enable the virtual host sudo a2ensite FlaskApp

Disabling the default apache2 page sudo a2dissite 000-default.conf

Create the .wsgi File
```
cd /var/www/FlaskApp
sudo nano flaskapp.wsgi 
```
Add the following code

#!/usr/bin/python
 import sys
 import logging
 logging.basicConfig(stream=sys.stderr)
 sys.path.insert(0,"/var/www/FlaskApp/")

 from FlaskApp import app as application
 application.secret_key = 'Add your secret key'
save and exit

Deploying flask app with apache2 is referred from Digital ocean

Installing require modules
You can either install all modules on your machine or create a virtual environment for the project and install the modules pip install flask sqlalchemy psycopg2 requests oauth2client

Setting up your Google Oauth2
Login to your developer console and select your project and edit OAuth details as following

Javascript origin http://ip.xip.io

redirect URI

http://ip.xip.io\login

http://ip.xip.io\gconnect

http://ip.xip.io\callback

xip.io is a free DNS which will be the same as using IP address

Final Step
Restart your apache2 server

sudo service apache2 restart
