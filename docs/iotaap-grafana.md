# IoTaaP - Grafana

Grafana is an open-source data visualization and monitoring tool that integrates with complex data from sources like Prometheus, InfluxDB, Graphite, and ElasticSearch. Grafana lets you create alerts, notifications, and ad-hoc filters for your data while also making collaboration with your teammates easier through built-in sharing features.

In this Guide we will install Grafana and InfluxDB.

## InfluxDB

[InfluxDB](https://en.wikipedia.org/wiki/InfluxDB) is an open-source time series database developed by InfluxData. It is written in Go and optimized for fast, high-availability storage and retrieval of time series data in fields such as operations monitoring, application metrics, Internet of Things sensor data, and real-time analytics.

We will install InfluxDB on our Ubuntu Linux machine.  Ubuntu users can install the latest stable version of InfluxDB using the ```apt-get``` package manager.

First we need to add InfluxData repository with the following commands:

```bash
wget -qO- https://repos.influxdata.com/influxdb.key | sudo apt-key add -
source /etc/lsb-release
echo "deb https://repos.influxdata.com/${DISTRIB_ID,,} ${DISTRIB_CODENAME} stable" | sudo tee /etc/apt/sources.list.d/influxdb.list
```
Then we can install and start InfluxDB service with the following commands:
```bash
sudo apt-get update && sudo apt-get install influxdb
sudo service influxdb 
```
By default InfluxDB will run on the port **8086**

## Creating InfluxDB database

Locally, InfluxDB will be available via command line using **influx** command. Start InfluxDB:

```bash
$ influx -precision rfc3339
Connected to http://localhost:8086 version 1.7.x
InfluxDB shell 1.7.x
>
```
Now create **weather_monitor** database:

```bash
> CREATE DATABASE weather_monitor
>
```
Show the databases with the following command:

```bash
> SHOW DATABASES
```
You should get the output like this:
```bash
> SHOW DATABASES
name: databases
name
----
_internal
weather_monitor
>
```
Influx statements must operate by using a specific database. We set our databse in the following way:

```bash
> USE weather_monitor
Using database weather_monitor
>
```
Now we will write some data to the database:

```bash
> INSERT temperature value=17
```
Repeat the command a few times with different values in order to add more data to the database, you can also add various variables like humidity, light, etc.

You can query for the data like this:

```bash
> SELECT * from "temperature", "humidity"
name: humidity
time                           value
----                           -----
2019-09-28T18:56:29.549596506Z 60
2019-09-28T18:56:30.847422308Z 60
2019-09-28T18:56:32.882847832Z 55
2019-09-28T18:56:36.302630615Z 57
2019-09-28T18:56:38.457372846Z 59

name: temperature
time                           value
----                           -----
2019-09-28T17:49:00.67317224Z  17
2019-09-28T17:51:44.980424695Z 17
2019-09-28T18:56:10.019263285Z 18
2019-09-28T18:56:12.639383076Z 17
2019-09-28T18:56:15.062209926Z 16
2019-09-28T18:56:16.919413288Z 17
>
```
After inserting some data we can proceed with the Grafana installation.

## Installing NGINX

Nginx is a web server which can also be used as a reverse proxy, load balancer, mail proxy and HTTP cache.

We have to setup Nginx in order to access apps from the web.

On Ubuntu, we can install Nginx with the following commands:

```bash
sudo apt update
sudo apt install nginx
```
We need to adjust the Firewall before using Nginx:

```bash
sudo ufw allow 'Nginx Full'
```
This will allow Nginx to use both port 80 and port 443

Check the status of your Web Server with the following command:

```bash
systemctl status nginx
```
And you should get the result like this:

```bash
● nginx.service - A high performance web server and a reverse proxy server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: active (running) since Sat 2019-09-28 21:04:28 CEST; 1min 22s ago
     Docs: man:nginx(8)
 Main PID: 3446 (nginx)
    Tasks: 2 (limit: 1152)
   CGroup: /system.slice/nginx.service
           ├─3446 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
           └─3449 nginx: worker process
           
```
Now we will check if our Nginx server is working properly…

Get your server’s IP address with the following command:

```bash
ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'
```
Point your browser to your IP address and you should see the default Nginx landing page:

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/iotaap-grafana/nginx-iot-default.png"nginx IoT Default")

Congrats! Your Nginx installation works, if you have any problems, feel free to use our [community page](https://community.iotaap.io/) and ask a question.

Now we just have to enable Nginx service to start up at boot:

```bash
sudo systemctl enable nginx
```

## Nginx server blocks

Nginx server blocks are similar to virtual hosts in Apache. Blocks are used for configuration details and hosting more than one domain on a single server.

Now we will create a directory for our domain **iotaap.cloud** (use your own domain here). Firstly we will create directory for our domain:

```bash
sudo mkdir -p /var/www/iotaap.cloud/html
```
Then we have to assign ownership to the directory:

```bash
sudo chown -R $USER:$USER /var/www/iotaap.cloud/html
```
We also have to ensure that permissions for our web are correct (755)

```bash
sudo chmod -R 755 /var/www/iotaap.cloud
```
Next, we will create a really simple web page, just to test our configuration:

```bash
vi /var/www/iotaap.cloud/html/index.html
```
with the following content (Edit if you want)
```markup
<html>
    <head>
        <title>Welcome to IoT Cloud</title>
    </head>
    <body>
        <h1>Great! Your IoT server is working!</h1>
    </body>
</html>
```
Save and close the file (with wq! command 🙂 )

Now we have to tell Nginx to serve our little web. We will create new configuration for our domain.

Create new fonfiguration file with the following command:

```bash
sudo vi /etc/nginx/sites-available/iotaap.cloud
```
and the following content:

```nginx
server {
        listen 80;
        listen [::]:80;

        root /var/www/iotaap.cloud/html;
        index index.html index.htm index.nginx-debian.html;

        server_name iotaap.cloud www.iotaap.cloud;

        location / {
                try_files $uri $uri/ =404;
        }
}
```
Now we have to enable our configuration by creating a symling to the sites-enabled directory:

```bash
sudo ln -s /etc/nginx/sites-available/iotaap.cloud /etc/nginx/sites-enabled/
```
We have to make a small adjustment in the nginx configuration:

```bash
sudo vi /etc/nginx/nginx.conf
```
And remove the # symbol before the line:

```nginx
http {

    server_names_hash_bucket_size 64;

}
```
Read more about ```server_names_hash_bucket_size``` [here](https://gist.github.com/muhammadghazali/6c2b8c80d5528e3118613746e0041263).

Test your Nginx configuration:
```bash
sudo nginx -t
```
You sould get this output:

```bash
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
If there are some problems go through the steps above again or [**ask the community**](https://community.iotaap.io/).

Finally, restart Nginx:

```bash
sudo systemctl restart nginx
```
Navigate to your domain in the browser, and you should se your test website:

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/iotaap-grafana/iot-cloud-nginx-website.png"IoT loud nginx website")

## Securing Nginx with Let's Encrypt

Let’s Encrypt is a Certificate Authority (CA) that provides an easy way to obtain and install free TLS/SSL certificates, thereby enabling encrypted HTTPS on web servers.

We have to install Certbot software on our server in order to use Let’s Encrypt.

We will add the repository:

```bash
sudo add-apt-repository ppa:certbot/certbot
```
Press _ENTER_ to accept.

Now install Certbot’s Nginx package:

```bash
sudo apt install python-certbot-nginx
```
Certbot is able to automatically configure SSL only if the correct server block is available in Nginx configuration. Certbot is looking for server_name directive that we set in the previous step.

Next we will obtain SSL Certificate **(use your domain here):**

```bash
sudo certbot --nginx -d iotaap.cloud -d www.iotaap.cloud
```
If this is your first time running ```certbot```, you will be prompted to enter an email address and agree to the terms of service. Next ```certbot``` will access the Let’s Encrypt server, and run a challenge to verify that you control the domain.

Next, certbot will ask you avout HTTPS settings:

```bash
Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
-------------------------------------------------------------------------------
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
new sites, or if you're confident your site works on HTTPS. You can undo this
change by editing your web server's configuration.
-------------------------------------------------------------------------------
Select the appropriate number [1-2] then [enter] (press 'c' to cancel):
```

Select your option – we suggest option 2, which will redirect al requests to the secure HTTPS.

Now reload your web using https:// and notice your browser’s security indicator.

**By default Let’s Encrypt’s certificates are only valid for 90 days.** But thanks to certbot we can automate certificate renewal process.

Certbot will add a new script to /etc/cron.d that will run twice a day and it will automatically reneq any certificate that’s within 30 days of expiration.

In order to test the reneqal process you can do a quick test:

```bash
sudo certbot renew --dry-run
```
If there was no errors, you are all set! Don’t worry about renewing your certificate(s) becatuse certbot will take care of that.

## Installing Grafana

We need to download and add GPG key:

```bash
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
```
and then we will be able to add Grafana repository to APT sources:

```bash
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
```
Refresh APT cache:

```bash
sudo apt update
```
Check that Grafana will be installed from the right repository:

```
apt-cache policy grafana
```
The version you are about to install will be at the top of the list, be sure that it looks something like this:

```bash
grafana:
  Installed: (none)
  Candidate: 6.3.6
  Version table:
     6.3.6 500
        500 https://packages.grafana.com/oss/deb stable/main amd64 Packages
```
Now you can start the installation:

```bash
sudo apt install grafana
```
After installation, start Grafana server:

```bash
sudo systemctl start grafana-server
```
Verify that Grafana is running:

```bash
sudo systemctl status grafana-server
```
Output should looks something like this:

```bash
● grafana-server.service - Grafana instance
   Loaded: loaded (/usr/lib/systemd/system/grafana-server.service; enabled; vendor preset: enabled)
   Active: active (running) since Sun 2019-09-29 00:54:28 CEST; 3s ago
     Docs: http://docs.grafana.org
 Main PID: 3266 (grafana-server)
    Tasks: 7 (limit: 1152)
```
Enable the service to start Grafana at each boot:

```bash
sudo systemctl enable grafana-server
```
**Next we have to set up the reverse proxy.** With the SSL certificate all data is secured by encrypting the connection, but now our Nginx is configured to serve our test website. In order to point all request to Grafana we have to make some changes.

Open your domain Nginx configuration file **(use your domain here):**

```bash
sudo vi /etc/nginx/sites-available/iotaap.cloud
```
Find the following block:

```bash
 location / {
        try_files $uri $uri/ =404;
    }
```
And change it to this:

```bash
    location / {
        proxy_pass http://localhost:3000;
    }
```
This will map the proxy to the port 3000 (Grafana runs on port 3000). You can test your configuration with:

```bash
sudo nginx -t
```
If everythign is ok, you have to reload Nginx:

```bash
sudo systemctl reload nginx
```
Now open your browser, point it to your domain and you should see the Login screen

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/iotaap-grafana/login.jpg"Grafana login")

Login with the following credentials:

Username: admin

Password: admin

Those are the initial credentials and on the next screen you will be prompted to change the default password. Enter some strong password and click _Save_.

Next, you will see the Home Dashboard

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/iotaap-grafana/grafana-initial-dashboard.jpg"Grafana initial dashboard")

Congrats! Everything works…

**In order to secure your Grafana instance, you have to Disable registrations adn anonymous access.**

Open Grafana configuration file:

```bash
sudo vi /etc/grafana/grafana.ini
```
Locate **allow_sign_up** directive:

```bash
[users]
# disable user signup / registration
;allow_sign_up = true
```
Remove the ; at the beggining of the line and set it to false, like this:

```bash
[users]
# disable user signup / registration
allow_sign_up = false
```
After that, locate the **enable** directive:

```bash
[auth.anonymous]
# enable anonymous access
;enabled = false
```
Remove the ; and set the directive to false, like this:

```bash
[auth.anonymous]
enabled = false
```
Save your file, exit the editor and restart Grafana:

```bash
sudo systemctl restart grafana-server
```
Once more, verify that Grafana service is working:

```bash
sudo systemctl status grafana-server
```

You should see the **active (running)** block, just like the before. Next, point your browser to your domain, Sign out and you will see that there is no Sign Up button, and that the only way to login is to enter your valid credentials.

That’s it, your own secured Grafana instance is now up and running!

If you have any questions feel free to discuss them with [**our community**](https://community.iotaap.io/).