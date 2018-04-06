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

