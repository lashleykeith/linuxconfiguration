# linuxconfiguration
A detailed linux configuration using Amazon Lightsail.

## Project Details
IP address: ```18.222.63.113/```(Will be deactived after review)

URL: http://18.222.63.113/
Web app is a modified version of [Flask Catalog](https://github.com/lashleykeith/MapApp)

## Amazon Lightsail Server Set Up
1. Login to you Amazon Web Console. https://signin.aws.amazon.com/signin?redirect_uri=https%3A%2F%2Fus-east-2.console.aws.amazon.com%2Fconsole%2Fhome%3Fregion%3Dus-east-2%26state%3DhashArgs%2523%26isauthcode%3Dtrue&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fhomepage&forceMobileApp=0

2.  Create a new AWS account and then go back to use the link above to log in

3.  Click 'Create Instance'

![1](https://user-images.githubusercontent.com/21030885/38419742-7f5bdd56-39dc-11e8-993b-d11a07731f41.jpg)

4.  Select the Linux/Unix platform and select the OS Only Ubuntu

![2m](https://user-images.githubusercontent.com/21030885/38419774-9d50cfce-39dc-11e8-8bc4-e473782db92b.jpg)

5.  Scroll to 'Name your instance' and click 'Create'
![1a](https://user-images.githubusercontent.com/21030885/38500930-b33bf284-3c46-11e8-9913-1219b3720db4.jpg)

6.  The instance needs a minute of two to start loading.  When it is reading you will see 'running' in the left corner of the status card.  Write down the public IP address on a paper for further use.

![2a](https://user-images.githubusercontent.com/21030885/38500937-b803a712-3c46-11e8-940c-588139b01b28.jpg)

7.  Click the status card and you come to this page.  Click the `Account page` link.
![3a](https://user-images.githubusercontent.com/21030885/38500946-bde2d2f2-3c46-11e8-8854-8d85028758cf.jpg)

8.  Click 'Download' to download your private key, it should go to your Download folder or your Document folder by default.  It is a .pem file
![6m](https://user-images.githubusercontent.com/21030885/38419959-3f5f581c-39dd-11e8-978d-e9cd738ba0c0.jpg)

**set up the aws instance**

9. Click the 'Networking' tab and find the 'Add another' at the bottom. Add port 123 and 2200. Amazon Lightsail allows only port 22 and 80 by default, no matter how you set it up in ubuntu's ufw

![4a](https://user-images.githubusercontent.com/21030885/38500953-c0b9f348-3c46-11e8-956a-b76a90ed9e3e.jpg)
  - From terminal on local machine
    - Rename download file `<filename>.pem` with `sudo mv <filename>.pem morekey.pem`
    - Move file to `.ssh` with `sudo mv morekey.pem ~/.ssh`
    - Change file permissions with `sudo chmod 600 ~/.ssh/morekey.pem`
    - Connect with SSH by entering `sudo ssh -i ~/.ssh/morekey.pem ubunutu@18.222.63.113`
    
10. Amazon Lightsail does not allow you to log in as a root user, but we can switch to become a root user. Type `$ sudo su -` and boom, we become a root user! Then type  `$ sudo adduser grader` to create another user 'grader'

11.  Now create a new file under the sudoers directory: `$ sudo nano /etc/sudoers.d/grader`. Fill that file with `grader ALL=(ALL:ALL) ALL`, then save it (control X, then type `yes`, then hit the return key on your keyboard)

12.  In order to prevent the `$ sudo: unable to resolve host` error, edit the hosts by `$ sudo nano /etc/hosts`, and then add `127.0.0.1 ip-172-26-14-97` under `127.0.0.1:localhost`

13.  Run the following commands to update all packages and install finger package:
- `$ sudo apt-get update`
- `$ sudo apt-get upgrade`
- `$ sudo apt-get install finger`

14.   Open a new Terminal window (Command+N) and input `$sudo ssh-keygen -f ~/.ssh/udacitykey.rsa`

15.  Stay on the same Terminal window, input `$ sudo cat ~/.ssh/udacitykey.rsa.pub` to read the public key. Copy the public key.

16.  Going back to the first terminal window where you are logged into Amazon Lightsail as the root user, move to grader's folder by `$ cd /home/grader`

17.  Create a .ssh directory: `$ mkdir .ssh`

18.  Create a file to store the public key: `$ touch .ssh/authorized_keys`

19.  Edit the authorized_keys file `$ nano .ssh/authorized_keys`

20.  Paste the public key you copied from the second terminal into the new `authorized_keys` file you made. 

20. Change the permission: `$ sudo chmod 700 /home/grader/.ssh` and `$ sudo chmod 644 /home/grader/.ssh/authorized_keys`

21. Change the owner from root to grader: `$ sudo chown -R grader:grader /home/grader/.ssh`

22. Restart the ssh service: `$ sudo service ssh restart`

23. Type `logout` to disconnect from Amazon Lightsail server

24. Log into the server as grader: `$ ssh -i ~/.ssh/udacitykey.rsa grader@13.58.225.150`

25. We now need to enforce the key-based authentication: `$ sudo nano /etc/ssh/sshd_config`. Find the *PasswordAuthentication* line and change text after to `no`. After this, restart ssh again: `$ sudo service ssh restart`

26. We now need to change the ssh port from 22 to 2200, as required by Udacity: `$ sudo nano /etc/ssh/ssdh_config` Find the *Port* line and change `22` to `2200`. Restart ssh: `$ sudo service ssh restart`

27. Disconnect the server by `$ ~.` and then log back through port 2200: `$ ssh -i ~/.ssh/udacitykey.rsa -p 2200 grader@13.58.225.150`

28. Disable ssh login for *root* user, as required by Udacity: `$ sudo nano /etc/ssh/sshd_config`. Find the *PermitRootLogin* line and edit to `no`. Restart ssh `$ sudo service ssh restart`

29. Now we need to configure UFW to fulfill the requirement:
- `$ sudo ufw allow 2200/tcp`
- `$ sudo ufw allow 80/tcp`
- `$ sudo ufw allow 123/udp`
- `$ sudo ufw enable`

**Now we are going to deploy the catalog application**

1. Install required packages
- `$ sudo apt-get install apache2`
- `$ sudo apt-get install libapache2-mod-wsgi python-dev`
- `$ sudo apt-get install git`

2. Enable mod_wsgi by `$ sudo a2enmod wsgi` and start the web server by `$ sudo service apache2 start` or `$ sudo service apache2 restart`.

3. Set up the folder structure (although clumsy, but works)
- `$ cd /var/www`
- `$ sudo mkdir catalog`
- `$ sudo chown -R grader:grader catalog`
- `$ cd catalog`

4. Now we clone the project from Github: `$ git clone [your link] catalog` Copy your link from here is the easiet way:

5. Create a .wsgi file: `$sudo nano catalog.wsgi` and add the following into this file
```
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/catalog/")

from catalog import app as application
application.secret_key = 'supersecretkey'
```

6. Rename the `application.py` to `__init__.py`

7. Now we need to install and start the virtual machine
- `$ sudo pip install virtualenv`
- `$ sudo virtualenv venv`
- `$ source venv/bin/activate`
- `$ sudo chmod -R 777 venv`

8. Now we need to install the Flask and other packages needed for this application
- `$ sudo apt-get install python-pip`
- `$ sudo pip install Flask`
- `$ sudo pip install httplib2`
- `$ sudo pip install sqlalchemy`
- `$ sudo pip install oauth2client`
- `$ sudo pip install sqlalchemy`
- `$ sudo pip install  psycopg2`
- `$ sudo pip install sqlalchemy_utils`
- `$ sudo pip install requests`
- `$ sudo pip install render_template`
- `$ sudo pip install redirect`
- `$ sudo pip install psslib`
- `$ sudo pip install --upgrader pip`
- `$ sudo pip install --upgrader --no-deps --force-reinstall utmp`



9. Use the `nano __init__.py` command to change the `client_secrets.json` line to `/var/www/catalog/catalog/client_secrets.json` and change the host to your Amazon Lightsail public IP address and port to 80

10. Now we need to configure and enable the virtual host
- `$ sudo nano /etc/apache2/sites-available/catalog.conf`
- Paste the following code and save

```
<VirtualHost *:80>
    ServerName 13.58.225.150
    ServerAlias MightyMax
    ServerAdmin grader@13.58.225.150
    WSGIDaemonProcess catalog python-path=/var/www/catalog:/var/www/catalog/venv/lib/python2.7/site-packages
    WSGIProcessGroup catalog
    WSGIScriptAlias / /var/www/catalog/catalog.wsgi
    <Directory /var/www/catalog/catalog/>
        Order allow,deny
        Allow from all
    </Directory>
    Alias /static /var/www/catalog/catalog/static
    <Directory /var/www/catalog/catalog/static/>
        Order allow,deny
        Allow from all
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    LogLevel warn
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

11. Now we need to set up the database
- `$ sudo apt-get install libpq-dev python-dev`
- `$ sudo apt-get install postgresql postgresql-contrib`
- `$ sudo su - postgres -i`

12. Now we create a user to create and set up the database. I name my database `catalog` with user `catalog`
- `$ CREATE USER catalog WITH PASSWORD [your password];`
- `$ ALTER USER catalog CREATEDB;`
- `$ CREATE DATABASE catalog WITH OWNER catalog;`
- Connect to database `$ \c catalog`
- `$ REVOKE ALL ON SCHEMA public FROM public;`
- `$ GRANT ALL ON SCHEMA public TO catalog;`
- Quit the postgrel command line: `$ \c` and then `$ exit`

13. use `sudo nano` command to change all engine to `engine = create_engine('postgresql://catalog:[your password]@localhost/catalog`

14. Initiate the database if you have a script to do so:
`cd /var/ww/catalog/catalog/`
`python database_setup.py`

15. Restart Apache server `$ sudo service apache2 restart` and enter your public IP address or host name into the browser. Hooray! Your application should be online now!

16. If it doesn't work go inside /var/www/catalog/catalog `sudo a2ensite catalog` and then try `sudo service apache2 restart`.
 
