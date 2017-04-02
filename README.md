# LinuxServer
Project deployed on AWS EC2 Instance

URL of Hosted Application: http://ec2-34-203-238-215.compute-1.amazonaws.com/

### Login to EC2 Instance
- Download private key and write down your public IP address
1. cd /Users/Desktop/key
2. chown 400 yourKeyPair.pem
3. ssh -i yourKeyPair.pem ubuntu@ip-address

## Update all installed software packages
1. sudo apt-get update
2. sudo apt-get update

## Create User
1. sudo adduser grader
2. Password: grader
3. Verify: grader
4. sudo visudo
- We add the following line after the comment line, “User privilege specification”:
-  grader   ALL=(ALL:ALL) ALL

### Add grader user to sudo groups
-  sudo adduser grader sudo
- Switch to grader user
- sudo su grader

## Generate KeyPairs
- ssh-keygen
- Enter file in which to save the key (/home/grader/.ssh/id_rsa): /home/grader/.ssh/grader
- /home/grader/.ssh/grader

## Create the SSH directory
- mkdir .ssh
- chmod 700 .ssh
- touch .ssh/authorized_keys
- chmod 600 .ssh/authorized_keys

## Edit the authorized_keys file with a text editor.
- cat ~/.ssh/authorized_keys
- nano .ssh/authorized_keys
- Paste public key from previous step

## Change SSH port
- sudo vim /etc/ssh/sshd_config
- change Port value to 2200
- Change PermitRootLogin from without-password to no
- Temporalily change PasswordAuthentication from no to yes.
- Append AllowUsers grader

# Configure Firewall
- sudo ufw default deny incoming
- sudo ufw default allow outgoing
- sudo ufw allow 2200/tcp
- sudo ufw allow www
- sudo ufw allow ssh
- sudo ufw allow ntp
- sudo ufw enable

## Install Git, Apache and mod-wsgi
- sudo apt-get install apache2 libapache2-mod-wsgi git
- sudo a2enmod wsgi

## Timezone
- sudo dpkg-reconfigure tzdata
- From the menu I selected 'None of the above' and then 'UTC.'

## Clone Item Catalog
- cd /var/www/
- sudo git clone https://github.com/juanv911/Restaurant.git

## Create WSGI Application
- sudo touch application.wsgi
<code>
<pre>
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/Restaurant/")

from project  import app  as application
application.secret_key = 'secret'
</pre>
</code>
#### Restart apache
- sudo service apache restart

## PostgreSQL
- sudo apt-get install postgresql postgresql-contrib
- sudo -i -u postgres
- createuser --interactive
- createdb catalog
- psql
- \password catalog

## Git
- sudo apt-get install git-all



