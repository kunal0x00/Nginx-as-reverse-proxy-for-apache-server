Step1: install apache2

![](media/image1.png){width="6.268055555555556in"
height="2.557638888888889in"}

Step 2: enabling apache2

![](media/image2.png){width="6.268055555555556in" height="1.00625in"}

Step 3: starting apache2

And try to finding on which port apache is running and it is running on
the port 80

![](media/image3.png){width="6.268055555555556in"
height="2.865972222222222in"}

For checking is it working properly or not I use my ip and enter the
<http://my_ip_Add> I got this page

![](media/image4.png){width="6.268055555555556in"
height="2.9520833333333334in"}

Step 4: I tried to make change the appearance of landing page of apache
so I did

Sudo nano /var/www/html/index.html

![](media/image5.png){width="6.268055555555556in" height="2.84375in"}

After that I given some user permission to apache website components

Using command :

Sudo mkdir --p /var/www/html/test

Sudo chown --R \$USER:\$USER /var/www/html/test

Sudo cp /var/www/html/index.html /var/www/html/test/

![](media/image6.png){width="6.276786964129484in"
height="1.918477690288714in"}

Now below it is showing that I changed the appearance of apache server
main page

![](media/image7.png){width="6.268055555555556in"
height="2.813888888888889in"}

And below image saying that this webpage running on apache webserver

![](media/image8.png){width="6.000837707786527in"
height="4.688153980752406in"}

So above details was for setting the apache webserver from scratch now
its time to do reverse proxy

Here I am installing nginx for making this as reverse
proxy![](media/image9.png){width="6.268055555555556in"
height="2.8756944444444446in"}

And after installation process complete I tried to run them

![](media/image10.png){width="6.268055555555556in"
height="1.3381944444444445in"}

Below command showing the status of nginx which is running

![](media/image11.png){width="6.268055555555556in"
height="2.236111111111111in"}

Same thing I did for apache2 , first I run apache by using command

Sudo systemctl start apache2.service

Below command showing the status of apache and it is also running

![](media/image12.png){width="6.268055555555556in" height="2.75625in"}

Now for sureity I checked both page by using there specific port number
apache 8080

And I made specific port for nginx 8180 for restrict confliction between
nginx and apache2

![](media/image13.png){width="6.268055555555556in"
height="2.4965277777777777in"}

![](media/image14.png){width="6.268055555555556in"
height="2.9652777777777777in"}

Now Its time to make nginx as the reverse server

So following command demonstrating the procedure

**Create Nginx Reverse Proxy Configuration**:

Create a new configuration file in the /etc/nginx/sites-available/
directory. For this example, we\'ll name it reverse-proxy.conf

**Add Reverse Proxy Configuration**:

I added the following configuration to this file.

![](media/image15.png){width="6.268055555555556in"
height="2.204861111111111in"}

**Enable the Configuration**:

Create a symbolic link from the configuration file in sites-available to
the sites-enabled directory:

![](media/image16.png){width="6.268055555555556in"
height="0.8729166666666667in"}

**Configure Apache to Listen on a Different Port**:

Edit the Apache configuration to listen on port 8080 instead of the
default port 80.

Open the Apache ports configuration file:

sudo nano /etc/apache2/ports.conf

I Changed the line that says Listen 80 to: Listen 8080

![](media/image17.png){width="6.268055555555556in"
height="3.123611111111111in"}

Next, update your virtual host configuration files (usually found in
/etc/apache2/sites-available/). For example, if you have a file named
000-default.conf, change:

\<VirtualHost \*:80\> to \<VirtualHost \*:8080\>

![](media/image18.png){width="6.268055555555556in"
height="3.8465277777777778in"}

**Restart Both Servers**:

Restart Apache and Nginx to apply the changes:

![](media/image19.png){width="6.268055555555556in"
height="2.1069444444444443in"}

Ok the reverse proxy is done for verifying the steup is nginx is working
as reverse proxy are not there are two methods for verifying

For verifying we have to identify our ip add

So ip add of my device is 192.168.110.137

![](media/image20.png){width="6.268055555555556in" height="4.5125in"}

Firt method is using curl to get the header details of page and it is
clearly showing that it is using nginx/1.26.0 as server

![](media/image21.png){width="6.268055555555556in"
height="2.7958333333333334in"}

And as I made earlier a apache webserver page here also when you inspect
your webpage you can get in the network section the page is using nginx
webserver as reverse proxy..

![](media/image22.png){width="6.268055555555556in"
height="2.963888888888889in"}

Here you can also see that nginx is as reverse proxy

![](media/image23.png){width="5.792474846894138in"
height="4.677736220472441in"}

Chatgpt:\-\--

**Verifying the Setup**

-   Open your web browser and navigate to your domain or IP address. You
    should see the content served by Apache, but the request is handled
    by Nginx as a reverse proxy.

-   You can also inspect the HTTP headers to confirm that Nginx is
    forwarding the requests to Apache.

**Summary**

-   **Create Nginx Configuration**: Add the reverse proxy settings to a
    new file in /etc/nginx/sites-available/reverse-proxy.conf.

-   **Enable Configuration**: Link the configuration file to
    /etc/nginx/sites-enabled/.

-   **Configure Apache**: Change Apache to listen on port 8080 and
    update virtual host files accordingly.

-   **Restart Servers**: Use sudo systemctl restart apache2 and sudo
    systemctl restart nginx.

This setup will allow Nginx to act as a reverse proxy for Apache,
forwarding requests from clients to your Apache server.
