# IoTaaP - Mosquitto

[Mosquitto](https://mosquitto.org/) is a popular MQTT server (or broker, in MQTT parlance) that has great community support and is easy to install and configure.

## Prerequisites

Before installing Mosquitto, we have to set up a host with real domain (example.com) and generate SSL certificate using Let’s Encrypt.

Installation and configuration steps can be found [here](https://www.iotaap.io/instructions/iotaap-grafana/#1569697278620-e4bf9a1a-d5cf).

## Installing Mosquitto

We can install Mosquitto from default Ubuntu software repository

```bash
sudo apt update
sudo apt install mosquitto mosquitto-clients
```
Ubuntu will start the Mosquitto service after install.

## Mosquitto Password

**mosquitto_passwd** is the utility that is used to generate a password. This command will place username and password pain in _/etc/mosquitto/passwd_

Let’s create new password for user **worker1**

```bash
sudo mosquitto_passwd -c /etc/mosquitto/passwd worker1
```
After pressing ENTER, you will have to enter your password, and confirm it.

Now we have to create a new configuration file for Mosquitto to define that we are using password file for all connections:

```bash
sudo vi /etc/mosquitto/conf.d/default.conf
```
Paste the following content to the file:

```bash
allow_anonymous false
password_file /etc/mosquitto/passwd
```
**Leave a trailing newline at the end of the file!**

Now, restart Mosquitto:

```bash
sudo systemctl restart mosquitto
```
Next, we will test our configuration. Open another SSH connection in your terminal, in order to have two connections side by side. We will use one connection to _publish_ data to the topic and another connection to _subscribe_ to the topic.

First connection is used for _subscribe_:

```bash
mosquitto_sub -h localhost -t topic/test -u "worker1" -P "password"
```
Leave this terminal open, it will listen for the new messages on the “topic/test” topic and publish some data to the topic from the second terminal:

```bash
mosquitto_pub -h localhost -t "topic/test" -m "Hi there!" -u "worker1" -P "password"
```
Now, open your first terminal and you shoud see “Hi there!” message received. Great, your configuration works! But it’s not safe because we are sending passwords unencrypted. In the next step we will configure SSL encryption. Be sure to configure your Nginx with the real domain before this step!

## Configuring MQTTS

MQTTS is actually abbreviation for MQTT + SSL, encrypted MQTT connection.

Once more, be sure to configure your Nginx with the real domain and obtain Let’s Encrypt SSL certificate, as described in the first step.

In order to use SSL we have to tell Mosquitto where Let’s Encrypt certificates are stored. We will edit our Mosquitto configuration once more:

```bash
sudo vi /etc/mosquitto/conf.d/default.conf
```
Add the following content just below your previous configuration and **leave a trailing newline at the end of the file**. Also, be sure to **use your domain here**, as we will use _iotaap.cloud_ domain:

```bash
listener 1883 localhost

listener 8883
certfile /etc/letsencrypt/live/iotaap.cloud/cert.pem
cafile /etc/letsencrypt/live/iotaap.cloud/chain.pem
keyfile /etc/letsencrypt/live/iotaap.cloud/privkey.pem
```
Port **1883** is the standard, unencrypted MQTT port, and this port is not exposed to the internet by default. Port **8883** is encrypted port that will be exposed to the internet.

Restart Mosquitto:

```bash
sudo systemctl restart mosquitto
```
Now we have to allow connections to port **8883**, also if you are using firewall on your server (AWS, DigitalOcean or similar) be sure to enable connections to this port, command below will allow connections in Ubuntu system:

```bash
sudo ufw allow 8883
```
Now, we can test our configuration using **mosquitto_pub** command, but with the following options:

to subscribe to our topic (use your domain here):

```bash
mosquitto_sub -h iotaap.cloud -t topic/test -u "worker1" -P "password" -p 8883 --capath /etc/ssl/certs/
```
This will subscribe you to the “topic/test”, next we will publish some data to the topic:

```bash
mosquitto_pub -h iotaap.cloud -t topic/test -m "Hi encrypted user :)" -p 8883 --capath /etc/ssl/certs/ -u "worker1" -P "password"
```
Open your first terminal and you should see the message “Hi encrypted user :)”. Congrats now you have your own Secured MQTT broker!

_–capath /etc/ssl/certs/_ enables SSL and tells to the **mosquitto_pub** to look for [root certificates](https://en.wikipedia.org/wiki/Root_certificate), Root certificates are installed by operating system, and the path is different for different OS.