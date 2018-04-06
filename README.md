# linuxconfiguration
A detailed linux configuration using Amazon Lightsail.

## Project Details
IP address: ```13.58.225.150/```(Will be deactived after review)

URL: http://13.58.225.150/
Web app is a modified version of [Flask Catalog](https://github.com/lashleykeith/MapApp)

## Amazon Lightsail Server Set Up
1. Login to you Amazon Web Console. https://signin.aws.amazon.com/signin?redirect_uri=https%3A%2F%2Fus-east-2.console.aws.amazon.com%2Fconsole%2Fhome%3Fregion%3Dus-east-2%26state%3DhashArgs%2523%26isauthcode%3Dtrue&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fhomepage&forceMobileApp=0

2.  Create a new AWS account and then go back to use the link above to log in

3.  Click 'Create Instance'

![1](https://user-images.githubusercontent.com/21030885/38419742-7f5bdd56-39dc-11e8-993b-d11a07731f41.jpg)

4.  Select the Linux/Unix platform and select the OS Only Ubuntu

![2m](https://user-images.githubusercontent.com/21030885/38419774-9d50cfce-39dc-11e8-8bc4-e473782db92b.jpg)

5.  Scroll to 'Name your instance' and click 'Create'
![3m](https://user-images.githubusercontent.com/21030885/38419792-b267820e-39dc-11e8-8356-b788fe99c1ff.jpg)

6.  The instance needs a minute of two to start loading.  When it is reading you will see 'running' in the left corner of the status card.  Write down the public IP address on a paper for further use.

![4m](https://user-images.githubusercontent.com/21030885/38419919-23427628-39dd-11e8-82c4-e6bf635ef801.jpg)

7.  Click the status card and you come to this page.  Click the `Account page` link.
![5m](https://user-images.githubusercontent.com/21030885/38419928-27ce0928-39dd-11e8-875f-44bddb3d6b1b.jpg)

8.  Click 'Download' to download your private key, it should go to your Download folder or your Document folder by default.  It is a .pem file
![6m](https://user-images.githubusercontent.com/21030885/38419959-3f5f581c-39dd-11e8-978d-e9cd738ba0c0.jpg)

9. Click the 'Networking' tab and find the 'Add another' at the bottom. Add port 123 and 2200. Amazon Lightsail allows only port 22 and 80 by default, no matter how you set it up in ubuntu's ufw
![7m](https://user-images.githubusercontent.com/21030885/38419975-45795784-39dd-11e8-947b-9241cc389ef6.jpg)
  - From terminal on local machine
    - Rename download file `<filename>.pem` with `mv <filename>.pem mightymax.pem`
    - Move file to `.ssh` with `mv mightymax.pem ~/.ssh`
    - Change file permissions with `chmod 600 ~/.ssh/mightymax.pem`
    - Connect with SSH by entering `ssh -i ~/.ssh/mightymax.pem ubunutu@13.58.225.150`
    
10. Amazon Lightsail does not allow you to log in as a root user, but we can switch to become a root user. Type `$ sudo su -` and boom, we become a root user! Then type  `$ sudo adduser grader` to create another user 'grader'

11.  Now create a new file under the sudoers directory: `$ sudo nano /etc/sudoers.d/grader`. Fill that file with `grader ALL=(ALL:ALL) ALL`, then save it (control X, then type `yes`, then hit the return key on your keyboard)

12.  In order to prevent the `$ sudo: unable to resolve host` error, edit the hosts by `$ sudo nano /etc/hosts`, and then add `127.0.1.1 ip-172-26-7-180` under `127.0.1.1:localhost`

13.  Run the following commands to update all packages and install finger package:
- `$ sudo apt-get update`
- `$ sudo apt-get upgrade`
- `$ sudo apt-get install finger`

14.   Open a new Terminal window (Command+N) and input `$ ssh-keygen -f ~/.ssh/udacitykey.rsa`

15.  Stay on the same Terminal window, input `$ cat ~/.ssh/udacitykey.rsa.pub` to read the public key. Copy the public key.

16.  Going back to the first terminal window where you are logged into Amazon Lightsail as the root user, move to grader's folder by `$ cd /home/grader`

17.  Create a .ssh directory: `$ mkdir .ssh`

18.  Create a file to store the public key: `$ touch .ssh/authorized_keys`

19.  Edit the authorized_keys file `$ nano .ssh/authorized_keys`

20. Change the permission: `$ sudo chmod 700 /home/grader/.ssh` and `$ sudo chmod 644 /home/grader/.ssh/authorized_keys`

17. Change the owner from root to grader: `$ sudo chown -R grader:grader /home/grader/.ssh`

18. Restart the ssh service: `$ sudo service ssh restart`

19. Type `$ ~.` to disconnect from Amazon Lightsail server
