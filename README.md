# setup-virtual-host-in-httpd
This setting will help you setup your server to host more than one site at time #setup with Joe#

## To do before setup
Install httpd and enable it
> sudo yum -y install httpd
> sudo systemctl enable httpd.service

Creating the directory structure
> sudo mkdir -p /var/www/example.com/public_html
> sudo mkdir -p /var/www/example2.com/public_html

Grant permission
> sudo chown -R $USER:$USER /var/www/example.com/public_html
> sudo chown -R $USER:$USER /var/www/example2.com/public_html
> sudo chmod -R 755 /var/www

Create Demo Pages for Each Virtual Host
Write your index demopage for test
> nano /var/www/example.com/public_html/index.html

make this sample html
 <html>
  <head>
    <title>Welcome to Example.com!</title>
  </head>
  <body>
    <h1>Success! The example.com virtual host is working!</h1>
  </body>
</html>

##### Do this again for example2.com public_html

Create virtualhost file
> sudo mkdir /etc/httpd/sites-available
> sudo mkdir /etc/httpd/sites-enabled

> sudo nano /etc/httpd/conf/httpd.conf

To the end of this file httpd.conf add this line: IncludeOptional sites-enabled/*.conf

Creating your first host example.com
> sudo nano /etc/httpd/sites-available/example.com.conf

inside this conf file write-out this: 

<VirtualHost *:80>

    ServerName www.example.com
    ServerAlias example.com
    DocumentRoot /var/www/example.com/public_html
    ErrorLog /var/www/example.com/error.log
    CustomLog /var/www/example.com/requests.log combined
    
</VirtualHost>

##### do it again to the example2.com

Finally enable them by creating it symbolic link in site_enabled
> sudo ln -s /etc/httpd/sites-available/example.com.conf /etc/httpd/sites-enabled/example.com.conf
> sudo ln -s /etc/httpd/sites-available/example2.com.conf /etc/httpd/sites-enabled/example2.com.conf

Lastly, restart your httpd
> sudo apachectl restart

##### Done configuring your virtualhost

Setup to localhost

> sudo nano etc/hosts

Add the following line in host:

127.0.0.1   localhost
server_ip_address example.com
server_ip_address example2.com

# Done! go and browse http://example.com
