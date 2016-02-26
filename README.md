# Item Catalog Server Configuration - [ajay]
A baseline installation of a linux server which hosts a web application using apache, python-flask and postgresql database server with scheduled automatic updates , configured firewalls, monitering and notification software against attacks.

### SSH [ Login Credentials]
please login to the server using 

``
ssh -i ~/location_of_private_key grader@52.36.207.14 -p 2200
``

### Url of the web application
The item-catalog project from  https://github.com/ajack13/item-catalog is hosted on the server  (Minor changes have been made to the code)

Please access the application by visiting the link given below

``
http://52.36.207.14
``
### Configuration changes and installed packages
Remote accesss of root user has been diabled . remote user grader can sudo to root

Key based ssh authentication is enforced 

SSH is hosted on non default port 2200

All packages have been updated to most recent updates

``
sudo apt-get update
``

``
sudo apt-get upgrade
``

local time-zone is configured to UTC

cron scripts have been added to automatically update packages by running update.sh .access crontab by typing  
``
crontab -e
``

firewall has been configured . to check firewall configuration type 

``
sudo ufw status
``

``
sudo ufw verbose
``

Glances package has been installed to monitor the server . access glances by typing 

``
glances
``

fail2ban and sendmail packages have been installed to monitor for attacks, unsuccessfull login attempts and notify the admin through mail 
,details can be found in 

``
/etc/fail2ban/jail.local
``

Apache web server is installed to host the web application along with python flask framework and mod_wsgi

Postgresql database has been installed and application data is stored in database user catalog is created with limited access with following commands

    CREATE USER catalog WITH PASSWORD '.....';
    ALTER USER catalog CREATEDB;
    CREATE DATABASE catalog WITH OWNER catalog;
    \c catalog
    REVOKE ALL ON SCHEMA public FROM public;
    GRANT ALL ON SCHEMA public TO catalog;
 
The application is hosted on port 80 the apache config is as below


        <VirtualHost *:80>
        DocumentRoot /var/www/item-catalog
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
        WSGIScriptAlias / /var/www/item-catalog/app.wsgi
        <Directory /var/www/item-catalog/>
                WSGIApplicationGroup %{GLOBAL}
                Order deny,allow
                Allow from all
        </Directory>
        </VirtualHost>


Git has been installed to clone project from git hub
        
        git clone https://github.com/ajack13/item-catalog.git
        
The packages installed for the app are as follows

        apt-get install postgresql python-psycopg2
        apt-get install python-flask python-sqlalchemy
        apt-get install python-pip
        pip install sqlalchemy-utils
        pip install oauth2client
        pip install requests
        pip install httplib2
        pip install dict2xml






