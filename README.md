# Welcome to my Linux-Server-Configuration Project

The purpose of this project is to configure a Linux virtual machine to host my Item Catalog web appliacation.

- You can visit http://34.235.117.81 to view the website in your browser.
- Accessible SSH Port: 2200
- IP address: 34.235.117.81
- Application URL: http://ec2-34-235-117-81.compute-1.amazonaws.com

## Let's get started! 
1. Create a user named grader and grant him/her sudo permission
  - SSH into the server through ssh -i ~/.ssh/udacity_key.rsa root@34.235.117.81
  - Create grader user sudo adduser grader
  - Create the a new file in the sudoers directory sudo nano /etc/sudoers.d/grader
  - Input the the following text grader ALL=(ALL:ALL) ALL
  - Type sudo nano /ect/hosts

2. Update all currently installed packages
  - sudo apt-get update 
  - sudo apt-get upgrade

3. Change SSH port from 22 to 2200 
- sudo nano /ect/ssh/ssh_config
- Changet the port from 22 to 2200
- Confirm by running ssh -i ~/.ssh/udacity_key.rsa -p 2200 root@34.235.117.81

4. Configure the UFW (Uncomplicated Firewall) to only allow incoming connections fro SSH port 2200, HTTP port 80 and NTP port 123
- sudo ufw allow 80/tcp
- sudo ufw allow 2200/tcp
- sudo ufw allow 123/udp
- sudo ufw allow enable

5. Configure the local timezone to UTC
- sudo dpkg-reconfigure tzdata and choose UTC

6. Configure key based authentication for grader user
- cp/root/.ssh/authorized_key /home/grader/.ssh/authorized_keys

7. Disable ssh login for root user 
- sudo nano /etc/ssh/sshd_config
- change PermitRootLogin without-password to PermitRootLogin no
- Restart ssh with sudo service ssh restart 
- Now you should be able to login with ssh -i ~/.ssh/udacity_key.rsa -p 2200 grader@34.235.117.81

8. Install Apache
- sudo apt-get install apache2

9. Install mod_wsgi
- sudo apt-get install libache2-mod-wsgi
- Enable mod_wsgi with sudo a2enmod wsgi
- start the server sudo service apache2 start 

10. Clone the Item Catalog app from Github
- sudo apt-get install git 
- cd/var/www/html
- sudo mkdir DirtbikeCatalog
- cd /DirtbikeCatalog
- clone my Item catalog project from https://github.com/RyGuy860/DirtbikeCatalog.git
- Create a catalog.wsgi file and add the following

11. Install Flask and other dependencies 
- Install pip sudo apt-get install python-pip
- Install Flask pip install Flask 
- Install dependencies sudo pip install httplib2 oauth2client sqlalchemy psycopg2

12. Update path of client_secrets.json file
- sudo nano __init__.py
- Change the client_secrets.json path to /var/www/html/DirtbikeCatalog/DirtbikeCatalog/client_secrets.json

13. Configure and enable a new virtual host 
- sudo nano /etc/apache2/sites-abailable/catalog.conf
- past the following code:

14. Install and configure PostgreSQL
- sudo apt-get install postgresql
- sudo su -postgres
- psql
- CREATE USER catalog WITH PASSWORD 'password';
- CREATE DATABASE catalog WITH OWNER catalog
- \c catalog
- Change create engint line in your __init__.py and database_setup.py to engine = create_engine('postgresql://catalog:password@localhost/catalog')
15. Restart Apache
- sudo service apache2 restart
