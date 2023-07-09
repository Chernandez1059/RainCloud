# RainCloud
Creating a personal file share cloud server using AWS and FileCloud.

The goal of this project is to make my own cloud server so that I can use it to grab or share files from anywhere.

-On AWS I selected EC2 for virtual servers within the cloud

-When launching the EC2 instance I Chose Filecloud for the AMI (Amazon Machine Image)

 ![Selecting Filecloud on AWS](https://github.com/Chernandez1059/RainCloud/assets/115307156/17748104-493a-4ddd-8f68-8f595c68c91b)
 
 -For the instance type I went with the t2.micro (Free Tier Eligible)
 
 -Created a new key pair

-Now that the instance is created we can take our Public IPv4 DNS and paste it into the address bar

 -Mine ended with compute.amazonaws.com
 
 -Before we hit enter we can do a forward slash (/) right after the url and enter "install", this will bring up a check list showing everything is working right.
 
 ![FileCloud Checks](https://github.com/Chernandez1059/RainCloud/assets/115307156/8be4edcf-32bc-405c-9db0-cd9e4cb82925)
 
 -If everything looks good we can delete "install" and replace it with admin (/admin)
 
 -This will bring up a login page and you will use the default login which is User:admin Pass:AWS Instance ID
 
 -Once logged in it will notify you to apply a valid FileCloud license file
 
 ![Valid License File](https://github.com/Chernandez1059/RainCloud/assets/115307156/716d0eab-a04e-4d9a-81c5-4eb308fb4b08)
 
 -We will grab our license from the FileCloud portal

-Now we are going to give it a domain name and an SSL using CloudFlare

 -In CloudFlare using your selected domain, go into DNS and choose Records
 
 ![Creating A New Record](https://github.com/Chernandez1059/RainCloud/assets/115307156/46c4008a-f2d2-481b-909a-f7be914faafc)
 
 -We are going to add a new record, it will be a subdomain, meaning it will be your network domain but it will have text before
 
 -Choose a name, this will be the text before your domain for example if you owned google it would be chosenname.google.com
 
 -Then take the public IPv4 from your AWS instance and insert it into the IPv4 Address box
 
 -![Creating A New Record(2)](https://github.com/Chernandez1059/RainCloud/assets/115307156/dcf5d056-3582-40ee-bfd0-82ca03c1abac)

-Now we will create a certificate signing request (CSR) for FileCloud

 -We will be following this (https://www.filecloud.com/supportdocs/fcdoc/latest/server/filecloud-administrator-guide/installing-filecloud-server/post-installation/ssl-configuration/use-ssl-on-linux/create-a-csr-for-filecloud)
 
 -You will want to generate the request from the terminal connected to your AWS instance
 
 -Once you have created you certificate request you should see two new files, server.csr & server.key
 
 -We will be using the server.csr so we will "cat server.csr" which will show our certificate request

-Now back at CloudFlare, we want to head over to the "SSL/TLS" section and into "Origin Server"

 ![Origin Server](https://github.com/Chernandez1059/RainCloud/assets/115307156/3176b243-a8ac-4387-8e0a-910c5cb9ea5d)
 
 -Here we will click "Create Certificate"
 
 -We are going to select "Use my private key and CSR", and this is where we are going to put in our certificate request from earlier
 
 -Put your hostnames into the area
 
 ![Selecting Filecloud on AWS](https://github.com/Chernandez1059/RainCloud/assets/115307156/fd4381e3-8712-4824-bd14-b2fa08df6496)
 
 -Then click create

-Once we do that we can install an SSL certificate on ubuntu

 -We will be following this (https://www.filecloud.com/supportdocs/fcdoc/latest/server/filecloud-administrator-guide/installing-filecloud-server/post-installation/ssl-configuration/use-ssl-on-linux/install-an-ssl-certificate-on-ubuntu)
 
 -After doing "sudo a2enmod ssl" we will grab the certificate file from the origin server on CloudFlare and paste it into a new file called server.crt
 
 -Then we will do the second set of commands
 
 -Then we will be skipping over to #4 and modifying our webserver config /etc/apache2/sites-enabled/000-default.conf using nano
 
 -Inside the config file, we will be changing our VirtualHost to 443 (HTTPS) adding "ServerName" where you will put your servers name and copy pasting the SSL Engine text from the guide under ServerName
 
 ![Webserver Config](https://github.com/Chernandez1059/RainCloud/assets/115307156/12a20839-ec22-43f8-b126-c1af25219472)
 
 -Finally we will restart apache "sudo systemctl restart apache2"

-Now you should be able to type in your url and it should take you to a login site with an with an SSL

![Succesful Site](https://github.com/Chernandez1059/RainCloud/assets/115307156/a77614b0-bdeb-482f-9090-c5f3c92c7d59)

-When first logging into Filecloud you get a notification warning you that you have an invalid server URL, we are going to change that URL in the settings from the default to our server name, check it, then hit save

-What We Accomplished

  -We successfully created an AWS EC2 instance and chose FileCloud as our AMI (Amazon Machine Image). Using Cloudflare, we registered a new subdomain and connected it to our domain. Next, we generated a Certificate Signing Request (CSR) within our 
EC2 instance and linked it to our domain. Finally, we installed an SSL certificate on our Ubuntu-based AWS instance, resulting in a successful cloud file share server.

