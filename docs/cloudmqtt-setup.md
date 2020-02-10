# CloudMQTT Setup

One of the most crucial IoT parts is the communication channel. One of the most common protocols used for IoT communication is MQTT (MQ Telemetry Transport protocol) which provides lightweight methods of carrying out messaging using publish and subscribe queueing model.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/publisher-subscriber.jpg"MQTT protocol")

MQTT is ideal for IoT, it is designed to be minimal and this is perfect to use it in mobility, embedded systems another bandwidth-sensitive application. MQTT introduces something called “MQTT Broker”, this is in the matter of fact the heart of publish/subscribe protocol, like MQTT is.

MQTT broker is responsible for receiving messages, filtering, determining who is subscribed to the message, and sending messages to subscribed clients (devices, phones, applications…) MQTT is based on TCP/IP.

[**Mosquitto**](https://mosquitto.org/) is one of the most widely used MQTT brokers. Mosquitto is free and open source.  [CloudMQTT](https://www.cloudmqtt.com/) uses Mosquitto. In the steps below we will go through the process of creating your CloudMQTT account (FREE) and testing your instance.

## CloudMQTT account

In this step we will setup our CloudMQTT free account for testing. Go to [CloudMQTT](https://customer.cloudmqtt.com/login) and create your account with your e-mail or using Google.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/cloudmqtt-login.jpg"CloudMQTT login")

## Creating new instance

In the upper right corner of your dashboard click on the button **“Create New Instance“**. Enter your project name, select **Cute Cat** free plan, and optionally add some tags to your project for easier navigation later. After finish, click on the **“Select Region”** button and proceed to the next step.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/cloudmqtt-create-instance-1-1024x503.jpg"CloudMQTT create instance")

## Select your region

In this step we will use EU-West-1 (Ireland) region. This is the region where CloudMQTT servers are based, and in free plan, you are able only to select between two regions. Click “Review” button, and your configuration should look something like this:

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/cute-cat-cloud-mqtt.jpg"Cute cat cloud MQTT")

If everything looks good, click on “Create instance“.

## Instance Data

If everything went well, you will see your instance up and running, click on it! You will be able to see your instance info and parameters that we will use to intereact with your instance.

Server is the server where you can access your instance.

User will be used to connect with your instance, as well as password.

Port is the port where your instance is running.

SSL port is secured port where your instance is running.

Your FREE instance can have 5 connections max.

## Testing with MQTT.fx

To test our instance, we will use **MQTT.FX**, this  is a MQTT Client written in Java based on [Eclipse Paho](http://www.eclipse.org/paho/). Download latest release of MQTT.fx from here. Select and download MQTT for your platform (Windows, Linux…). Run your setup and install it. After running MQTT.fx, click on **Edit Connection Profiles**, cog, next to the Connect button.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/mqtt-fx-1024x584.jpg"MQTT FX")

This is the place where we will add our new MQTT connection. Simply click on the **+** button in the bottom left corner and fill your new profile with the data from CloudMQTT instance info.

Your new connection profile should look something like this:

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/mqtt-new-connection-1024x741.jpg"MQTT New connection")

**Do not forget to enter your User and Password in “User Credentials” tab!**

Click Apply and close the widow. **Next** is to connect to our MQTT instance, select your instance from the dropdown menu, and click connect.

In the next step we will publish our first topic!

## Publishing topic

In this step we will publish our first topic.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/mqtt-cloud-publish-topic.jpg"MQTT Publish new topic")

We have to write our topic and a message. Let’s say that we want to turn on our kitchen light. As you can se we will write **/kitchen/light** and this will be our topic, for example **/kitchen/fridge** could be our fridge topic.

Important things about MQTT topic names:

**Case sensitive**

**Use UTF-8 strings.**

**Must consist of at least one character to be valid.**

Click on the **Publish** button. Open your CloudMQTT dashboard, select your instance and click on **CONNECTIONS** tab. You will se that one connection is now listed there. Congrats! Your connection works!

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/mqtt-cloud-connections.jpg"MQTT Cloud connections")

## Subrscribing

Now when we are sure that our connection works and that we can publish, we will subscribe our “client” to the topic.

Open your **MQTT.fx** and select **Subscribe** tab, next to the Publish tab. We will subscribe to our ‘kitchen light’ topic. Write **“/kitchen/light”** to the subscribe field and click **Subscribe**. You will see your topic listed.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/mqtt-subscribe.jpg"MQTT Subscribe")

Next, we will publish to our topic as mentioned in the previous step. Click back on the **Subscribe** tab, and you will see your topic data received in the left window, as well as your topic’s data (ON). Try to change your message data and publish it again, also, you can play by adding various topics (e.g. /garden/fountain/water or /garden/fountain/light).

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/mqtt-fx-received-subscribed-topic-1024x367.jpg"MQTT Subscribe topic")