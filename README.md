# Udacity-Linux-Server-Configuration
### By Ratna kumari Kunisetti

### About
  This is the Udacity project 6 about the Configuring the Linux the server.

### Server Details
Server IP Address 13.126.48.184

port : 2200

Hosted site Url [http://13.126.48.184.xip.io/](http://13.126.48.184.xip.io/)

### Configuring Linux ServerLogin to the grader

# Login to the grader
First open puttygen and then load the lightsail1.pem to genarate private key

It is saved as privatekey.ppk

Now open putty give the static ip:13.126.48.184 and port:2200

Now go to ssh and in ssh goto to auth load the privatekey

login as : ubuntu

su - grader

password for grader:sai

# Making .ssh folder
mkdir .ssh

creates .ssh directory

touch .ssh/authorized_keys

create authorized_keys in .ssh folder

nano .ssh/authorized_keys

In git bash ssh-keygen

.ssh file will be created with id_rsa files

now copy the contents of id_rsa.pub file (microsoft office file) and paste it in the grader.

#### Updating all packages
```
sudo apt-get update
sudo apt-get upgrade
```

#### Creating grader User:
  ```
  sudo adduser grader
  ```
  This will add new user
  Password for this user : sai
  ```
  sudo nano /etc/sudoers
  ```
  Below the Root user append the following line
  ```
  grader  ALL=(ALL:ALL) ALL
  ```
  This will grant sudo permission to grader
  #### Creating a ssh key pair for grader
   On your local machine in terminal/command prompt
   ```
   ssh-keygen
   ```
   This will generate public and private ssh keys which is saved to .ssh folder
   
   Then in your virtual machine change to newly created user
   ```
   su - grader
   ```
   Create a new directory .ssh and new file authorized_keys in that directory
   ```
   mkdir .ssh
   sudo nano .ssh/authorized_keys
   ```
   Copy the public key with .pub extension to authorized_keys and save the file
   ```
   chmod 700 .ssh
   chmod 644 .ssh/authorized_keys
   ```
   - 700 will give read write and execute permission to user.
   - 644 prevent other user from writting in to file.
   Then restart ssh server
   ```
   service ssh restart
   ```
   
   Now from your log in to grader with private key generated 
   ```
   ssh -i .ssh/id_rsa grader@13.126.48.184
   ```
  #### Changing the ssh port to 2200
   ```
   sudo nano /etc/ssh/sshd_config
   ```
   Change port 22 to port 2200
    
   Restart the ssh server
   
   ```
   service ssh restart
   ```
   
   >Note: Before Logging using ssh add custom TCP port 2200 under lightsaail firewall in networking tab in lightsail instance console  
   
   Now Login using command like this
   ```
   ssh -i .ssh/id_rsa -p 2200 grader@13.126.48.184
   ```
   
  #### Disabling ssh login as root
  `sudo nano /etc/ssh/sshd_config`
  
  make change `PermitRootLogin no`
  
  #### Configurating  Ufw firewall

  Goto amazon aws lightsail account 

  Goto networking then change firewall as follows
  ```
  Custom        UDP        123
  
  Custom        TCP        2200
  
  Configurating Ufw firewall
    sudo ufw allow 2200/tcp
    sudo ufw allow 80/tcp
    sudo ufw allow 123/udp
    sudo ufw enable
This will allow all required ports and enables the ufw

After that

   sudo ufw status
It will display all allowed ports
  ```
  
  #### Installing Apache2 
  In terminal 
  
  ```sudo apt-get install apache2```
  
  Now mod_wsgi
  
  ```sudo apt-get install python-setuptools libapache2-mod-wsgi```
  
  Enable mod_wsgi
  
  ```sudo a2enmod wsgi ```
  ##### Setting up your flask application to work with apache2
   Creating a flask app
   
   In /var/www directory create a new folder
   `sudo mkdir FlaskApp`
   
   Install git 
   
   `sudo apt-get install git`
   
   move to the FlaskApp `cd FlaskApp`
   
   In that direcory clone your github repository
   
   `sudo git clone  'https://github.com/ratnakumarikunisetti/buid-itemcatalog1.git'`
   
   Renaming the repository to FlaskApp
   
   Then rename app.py file to `__init__.py`
   
   Make Following changes in __init__.py
   ```
   PROJECT_ROOT = os.path.realpath(os.path.dirname(__file__))
   json_url = os.path.join(PROJECT_ROOT, 'client_secrets.json')
   CLIENT_ID = json.load(open(json_url))['web']['client_id']
   ```
   Use json_url instead client_secrets.json in script
   
   
  ##### Install and configuring postgresql for project
   Install Postgres `sudo apt-get install postgresql`
   
   login to postgres `sudo su - postgres`
   
   postgres shell `psql`
   
   create user `CREATE USER catalog WITH PASSWORD 'password';`
   
   permit user to createdb `ALTER USER catalog CREATEDB;`
   
   Create a db name  catalog with user catalog `CREATE DATABASE catalog WITH OWNER catalog;`
   
   connect to db `\c catalog`
   
   revoke all permission to public `REVOKE ALL ON SCHEMA public FROM public;`
   
   Give schema permission to user catalog `GRANT ALL ON SCHEMA public TO catalog;`
   
   exit from db and postgres `\q and exit`
   
   Change the database connection in both database_setup.py and `__init__.py` as `engine =       create_engine('postgresql://catalog:password@localhost/catalog')`
   
   Now we are ready with our applicatiom
  #### Configure and Enable a New Virtual Host
   `sudo nano /etc/apache2/sites-available/FlaskApp.conf`
   
   In this add the following code
   ```
   <VirtualHost *:80>
		ServerName 13.126.48.184
		ServerAdmin admin@mywebsite.com
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
   ```
   Enable the virtual host 
   `sudo a2ensite FlaskApp`
   
   Disabling the default apache2 page
   `sudo a2dissite 000-default.conf`
   
  #### Create the .wsgi File
    ```
    cd /var/www/FlaskApp
    sudo nano flaskapp.wsgi 
    ```
   Add the following code
   
   ```
   #!/usr/bin/python
    import sys
    import logging
    logging.basicConfig(stream=sys.stderr)
    sys.path.insert(0,"/var/www/FlaskApp/")

    from FlaskApp import app as application
    application.secret_key = 'Add your secret key'
   ```
   save and exit
   
   Deploying flask app with apache2 is referred from [Digital ocean]
   (https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)
   
   #### Installing require modules
   You can either install all modules on your machine or create a virtual environment for the project and install the modules
   ` pip install flask sqlalchemy psycopg2 requests oauth2client`
   
   #### Setting up Google Oauth2
   
   - Go to [Google Cloud Plateform](https://console.cloud.google.com/).
  
   - Click `APIs & services` on left menu.
   
   - Click `Credentials`.
   
   - Create an OAuth Client ID (under the Credentials tab), and add http://13.126.48.184.xip.io 
     as authorized JavaScript origins.
   
   - Add the below three links as authorized javascript origins
     
     1. http://13.126.48.184.xip.io/

     2. http://13.126.48.184.xip.io/gconnect

     3. http://13.126.48.184.xip.io/callback 

   - Download the corresponding JSON file, open it et copy the contents.

   - Open `/var/www/FlaskApp/FlaskApp/client_secret.json` and paste the previous contents into the this file.

   - Replace the client ID in app.js file in the project directory.
   
   #### Final Step
   
   Restart apache2 server
  
   `sudo service apache2 restart
  #my lightsaildefaultprivate key-ap-south-1.pem`
   -----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAwBQAKY1XXBdlba92c4vvIzeC6mj7qtqQRjCFyRm7SU7McTe+
bLw1rCO9abxm+mPD3gB6dEGru4RmuZpvEjj/JhsXk9nq+PCtrRzvuKyPHF5AqUK3
Ju0WLovN3F8fRbRMwbg80aF6YLwDBJu1Xml6MztiaKazhsN8UWsJgwM05L7IvqBV
8tC6qfCOPjgq/ya6VovuQc+Y9xp0jOUws/XWNK/6f585UfXRzZOxdsiZyOIagkqp
3wsNhsuboerthzlsIWiFyOZ9f3DgztdtVt10cNfhC6bxAYFhJr25xokZhOnvnjoy
rmiFDtsr/YgPa7ltzJCtDhQYszlB8IIa0ZSoGQIDAQABAoIBACUKNlXM+dG6eUbD
lVYG4CCsbcSCZjW2XCgM51+2ZJVoaqhSlZgmWztM0RP4zuruHjFLalHM8C8DA0Q7
cbvT3fAdPVi3p0ZGM1e0ws2cCSTxUArT4LnS8nobX6FlfoRUlpCs1J2gRBcvam2T
EVCZioUiqIGB1BDi1bBlsbnf/m01v4Xm66tyUXmRh97u27jSBrG6YyIe+IKEC9Fk
Lg8D/gzwIHTt+YUhjsdSzbr1GONw5q0jmKWmD9PdVCO8ZXv8N03TyNcd0EzY5sSN
s4BycPBUmfoDP2rjy4YOHo8bYf4ZsSbjbSiD9lqFlYQ2tEaJPt0rhROgBas4z2d1
34zMVeECgYEA7kBO3YPkaNsozENJbP088K9ZVKf3wx2MGI8ri63+sh8RO2pE3Z9U
9zubcqjK7PVZJfHDIFUXjZEZM8kSy/+r/95k37xOAVaKNT+KZXNywoGuKUf/YLfT
WdKUwJ+3hJwXobW1WkY16fu+JR9K7gYYHDGUwjFhmjveOQwB7L1aioUCgYEAzmMg
Dbcd+DIjPzIYnn688y8D1LLgvEGU4AiVJ158FQkOvGNfK4vV98MUYV3qeaXXQ5qm
8GAgrqHFhw+Q7OVOE0t/9/z8KHzRsT4al2Gn11o0+WHpu4MaMHPEaH8OfZWnvZcT
nH1nVfM7KuNWw95Tyh9U1oBjEBWWsoORf701PYUCgYBAHBfcrZfxyz9gL577b+1N
CrIsAILAAxxmo2fhTzGg9pEpfsAHLs+rM2Px54+rUZ3qgvKxqZQL6QZyE+I1+Jds
44gbWE1ZONM53t47zGQOCN03iIMkoHKD0hFq/89fJK2LOx0QrKHnU3FoBdKg2Az2
0TSpSKZt3Tw/94YxEQbjDQKBgQCzC82n+F94jU6EqZowDfUv526kXJaY2zAjd26m
G9L7kMMG7hKHPaXfbo7EtWwQIq5wSL9gs5RGy7MIK5nn2jp0hMA8zG2ZVke4Qw9g
mui369sfKjFSajcTJ6uRmABjNKyzzlfGIAjAyOVgnJ8OB1ebdrjr6a+HKaN1tKxK
LEP+3QKBgFPc+9JUVqYX4kB/NoGtuwPsLv97zmWNqtbzNbt8DRpMmrBtumwMWvle
DKB3h3kC6FcatRmoEJ1QMQezpK+ZOOjemRNTbmffzBJlSVZvcaWUhU1lOS3a69C/
SZPAW6OWnoEif0eqrAK8n/GJ5l4QdoW62cMfYuwJP9Mw+tZcpiew
-----END RSA PRIVATE KEY-----




# Privatekey.ppk file

PuTTY-User-Key-File-2: ssh-rsa
Encryption: none
Comment: imported-openssh-key
Public-Lines: 6
AAAAB3NzaC1yc2EAAAADAQABAAABAQDAFAApjVdcF2Vtr3Zzi+8jN4LqaPuq2pBG
MIXJGbtJTsxxN75svDWsI71pvGb6Y8PeAHp0Qau7hGa5mm8SOP8mGxeT2er48K2t
HO+4rI8cXkCpQrcm7RYui83cXx9FtEzBuDzRoXpgvAMEm7VeaXozO2JoprOGw3xR
awmDAzTkvsi+oFXy0Lqp8I4+OCr/JrpWi+5Bz5j3GnSM5TCz9dY0r/p/nzlR9dHN
k7F2yJnI4hqCSqnfCw2Gy5uh6u2HOWwhaIXI5n1/cODO121W3XRw1+ELpvEBgWEm
vbnGiRmE6e+eOjKuaIUO2yv9iA9ruW3MkK0OFBizOUHwghrRlKgZ
Private-Lines: 14
AAABACUKNlXM+dG6eUbDlVYG4CCsbcSCZjW2XCgM51+2ZJVoaqhSlZgmWztM0RP4
zuruHjFLalHM8C8DA0Q7cbvT3fAdPVi3p0ZGM1e0ws2cCSTxUArT4LnS8nobX6Fl
foRUlpCs1J2gRBcvam2TEVCZioUiqIGB1BDi1bBlsbnf/m01v4Xm66tyUXmRh97u
27jSBrG6YyIe+IKEC9FkLg8D/gzwIHTt+YUhjsdSzbr1GONw5q0jmKWmD9PdVCO8
ZXv8N03TyNcd0EzY5sSNs4BycPBUmfoDP2rjy4YOHo8bYf4ZsSbjbSiD9lqFlYQ2
tEaJPt0rhROgBas4z2d134zMVeEAAACBAO5ATt2D5GjbKMxDSWz9PPCvWVSn98Md
jBiPK4ut/rIfETtqRN2fVPc7m3Koyuz1WSXxwyBVF42RGTPJEsv/q//eZN+8TgFW
ijU/imVzcsKBrilH/2C301nSlMCft4ScF6G1tVpGNen7viUfSu4GGBwxlMIxYZo7
3jkMAey9WoqFAAAAgQDOYyANtx34MiM/MhiefrzzLwPUsuC8QZTgCJUnXnwVCQ68
Y18ri9X3wxRhXep5pddDmqbwYCCuocWHD5Ds5U4TS3/3/PwofNGxPhqXYafXWjT5
Yem7gxowc8Rofw59lae9lxOcfWdV8zsq41bD3lPKH1TWgGMQFZayg5F/vTU9hQAA
AIBT3PvSVFamF+JAfzaBrbsD7C7/e85ljarW8zW7fA0aTJqwbbpsDFr5Xgygd4d5
AuhXGrUZqBCdUDEHs6SvmTjo3pkTU25n38wSZUlWb3GllIVNZTkt2uvQv0mTwFuj
lp6BIn9HqqwCvJ/xieZeEHaFutnDH2LsCT/TMPrWXKYnsA==
Private-MAC: 0d1c91e8b7ace4358510a40814ac15085f72bb52


# id_rsa file

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

# id_rsa.pub file

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC1W8IwQCZTAxYtnMnQ8uTSMy2vE/E0bc7DhsoOCNeJSogjHqDvpQk92W0M4i+6YI1wt28yJoUaWFTZXoIk7DJuyq7o1+u2NwezkW++V04/OaoqC1k2TmV3tWrRWp9ZHqr+/CNgcsosAf2G59BtE2FuW4H7QojqrEN9xp4Z6GbNWHSF8HZabx8CRlX+wirdCSEaq+LUubbMC6h4FaLOIhP2jbKtBADxLuN7SRpemmucJgEFbGX0YxVwPKdtnkj9txOjUSK00X8t+SJGGo390NVaIVXfnPBw3QuMmktmtWLxHy5VzIp4h1dD6vrmBQqcugrWkcVrsmlNR9SDPSXNd9ef hp@DESKTOP-E82NM60
#known_hosts
13.126.48.184 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBAHsWrX3KV+ODfnXh2UtP4QuQuobIjaI3iBeKBRLlgcYXf6NGbcaeF5Va/8n9ASbEapWRatUcCSu1O4GMV51QvQ=#
## References

   1. https://github.com/rrjoson/udacity-linux-server-configuration

   2. https://github.com/boisalai/udacity-linux-server-configuration

   3. https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps


