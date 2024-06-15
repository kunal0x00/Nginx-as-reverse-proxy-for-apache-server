Step1: install apache2  
![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/ec50aec9-b0c7-4e34-aec5-cb1e5fc88299)



Step 2: enabling apache2 
![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/403d42ec-c88d-4f26-af84-cfc9a7ec364f)



Step 3: starting apache2 

And try to finding on which port apache is running and it is running on the port 80 

![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/3038bc55-b45a-4d6b-ac2a-5fd09928b1f8)


For checking is it working properly or not I use my ip and enter the[ http://my_ip_Add ](http://my_ip_add/)I got this page 

![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/fb99a44f-a0da-465b-8e15-b774a1e988e5)


Step 4: I tried to make change the appearance of landing page of apache so I did  Sudo nano /var/www/html/index.html  

![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/63845a5e-f4ee-4af1-a458-cd851d43ac5c)


After that I given some user permission to apache website components Using command : 

Sudo mkdir –p /var/www/html/test 

Sudo chown –R $USER:$USER /var/www/html/test 

Sudo cp /var/www/html/index.html /var/www/html/test/ 

![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/fec4bd49-17d7-4d60-9352-5b12a37320d5)


Now below it is showing that I changed the appearance of apache server main page  

![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/b473bef5-36e6-4320-b7f5-a6e002afcab4)


And below image saying that this webpage running on apache webserver  

![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/001bcb85-03d9-45f5-b709-ae16e4410d6a)


So above details was for setting the apache webserver from scratch now its time to do reverse proxy  

Here I am installing nginx for making this as reverse proxy

![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/2a60a797-a9ac-436b-a8e8-b891d5d4f3d9)


And after installation process complete I tried to run them  

![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/a90150af-f56a-4f35-97d8-aab4ddf5903f)


Below command showing the status of nginx which is running  

![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/624a8b75-77e3-4df3-b94d-59f55b32487e)


Same thing I did for apache2 , first I run apache by using command  Sudo systemctl start apache2.service 

Below command showing the status of apache and it is also running 

![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/7c9756a6-f182-4b31-869a-36c358fe0049)


Now for sureity I checked both page by using there specific port number apache 8080  And I made specific port for nginx 8180 for restrict confliction between nginx and apache2 

![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/5f7e71e1-f8de-42af-9b43-da865f848614)

![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/7addbad1-7dc2-4840-b9ed-97a916263d94)


Now Its time to make nginx as the reverse server  

So following command demonstrating the procedure  

**Create Nginx Reverse Proxy Configuration**: 

Create a new configuration file in the /etc/nginx/sites-available/ directory. For this example, we'll name it reverse-proxy.conf 

**Add Reverse Proxy Configuration**: 

I added the following configuration to this file. 

![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/3d9dac86-ba9d-4909-8803-04b120be1d44)


**Enable the Configuration**: 

Create a symbolic link from the configuration file in sites-available to the sites- enabled directory: 

![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/1d37cd83-a22b-4550-9946-50c5858dfb7b)


**Configure Apache to Listen on a Different Port**: 

Edit the Apache configuration to listen on port 8080 instead of the default port 80. Open the Apache ports configuration file: 

sudo nano /etc/apache2/ports.conf 

I Changed the line that says Listen 80 to: Listen 8080

![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/a75d1bb4-2cfa-42ed-b0c8-70591d371723)


Next, update your virtual host configuration files (usually found in /etc/apache2/sites- available/). For example, if you have a file named 000-default.conf, change: 

<VirtualHost \*:80> to <VirtualHost \*:8080> 

![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/8ab27563-54ee-43e5-8275-12b8d18d76e0)


**Restart Both Servers**: 

Restart Apache and Nginx to apply the changes: 

![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/38e8cef0-2022-4212-9876-daf353041caa)


Ok the reverse proxy is done for verifying the steup is nginx is working as reverse proxy are not there are two methods for verifying  

For verifying we have to identify our ip add  So ip add of my device is 192.168.110.137 

![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/95a4419a-fa06-402d-a889-cee1143e035c)


Firt method is using curl to get the header details of page and it is clearly showing that it is using nginx/1.26.0 as server  

![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/e2dd1f02-24bc-474e-ab70-a3d1847bd56e)
And as I made earlier a apache webserver page here also when you inspect your webpage you can get in the network section the page is using nginx webserver as reverse proxy.. 

![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/89c191d9-4f2d-45b1-b7d8-af93022face2)


Here you can also see that nginx is as reverse proxy  

![image](https://github.com/kunal0x00/Nginx-as-reverse-proxy-for-apache-server/assets/117434806/2b5df175-ba5c-424f-a01e-6510035bfaef)

Chatgpt:--- 

**Verifying the Setup** 

- Open your web browser and navigate to your domain or IP address. You should see the content served by Apache, but the request is handled by Nginx as a reverse proxy. 
- You can also inspect the HTTP headers to confirm that Nginx is forwarding the requests to Apache. 

**Summary** 

- **Create Nginx Configuration**: Add the reverse proxy settings to a new file in /etc/nginx/sites-available/reverse-proxy.conf. 
- **Enable Configuration**: Link the configuration file to /etc/nginx/sites-enabled/. 
- **Configure Apache**: Change Apache to listen on port 8080 and update virtual host files accordingly. 
- **Restart Servers**: Use sudo systemctl restart apache2 and sudo systemctl restart nginx. 

This setup will allow Nginx to act as a reverse proxy for Apache, forwarding requests from clients to your Apache server.
