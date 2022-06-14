<h1 color=gray > Configure postfix in Ubuntu
<img src="https://www.espaciolinux.com/wp-images/articulos/2021/05/logo_postfix.jpg" align=right width=30%>
</h1>

***Prerequisites***

- 🚧 A domain name
- 🌏 NS hosting
- 🧭 Static public IP
- 🚩 Ubuntu server

<br>



***How to configure it***

This is a step by step to help you set up a postfix server. 

Use case: To work with Gophish.


##### 1. Make sure your system is updated, run:
  >sudo apt-get update

  >sudo apt-get upgrade

##### 2.  Run this to get to know your hostname.
>hostnamectl


##### 3. In case your domain name is not placed yet, you might set it with:
>hostnamectl set-hostname mydomain.com


##### 4. Open your /etc/hosts file to assign your system´s public IP to your domain
>sudo nano /etc/hosts

You will need admin privilege to edit the file

##### 5. You will see your localhost at 127.0.0.1 but you will need to add two more lines:

- 127.0.0.1 mail.mydomain.com   (This will work as your MX record in your NS zone.)
- [your public IP add] mydomain.com (This will work as your A record in your NS zone.)

Save your changes and exit the prompt.

##### 6. Go to your domain hosting and set an A and MX record.  Your _A_ will be your static public IP and your _MX_ will be mail.mydomain.com

##### 7. Install Postfix and mail package mailutils.
  >sudo apt install postfix mailutils

  In case it´s the first time working with Postfix, you will see a config prompt that will help you to configure it so select _Internet site_, then write down your domain name and tap _OK_.

##### 8. Check and edit (only if needed) the main.cf file at nano /etc/postfix/main.cf

  What should you look for?
- $myhostname: check your hostname is the same as the hostname you set at the beginning of this process. By this parameter, you tell postfix where it resides and could be the same as your domain name.
- $mydomain: should be your domain name, since it can be the same parameter as $myhostname, it is not necessary to have it.
- $mynetworks: if you use another service, IP, server to use/call your mailserver to deliver emails, you have to add it. For example: mynetworks= 127.0.0.0/8 *69.63.176.13 192.168.10.0/24* [::ffff:12] .....

Postfix´s config will be stored in main.cf file, all the parameters to make the service run will be found there. You can change them anytime you need.

##### 9. Save main.cf and any other postfix file to restart the service by:
>systemctl restart postfix

##### 10. Let´s test the service by sending and email.
>echo "Body of the mail" | mail -s "Subject" mail@server.com
