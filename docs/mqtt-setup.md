# MQTT Setup

One of the most crucial IoT parts is the communication channel. One of the most common protocols used for IoT communication is MQTT (MQ Telemetry Transport protocol) which provides lightweight methods of carrying out messaging using publish and subscribe queueing model.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/publisher-subscriber.jpg"MQTT protocol")

MQTT is ideal for IoT, it is designed to be minimal and this is perfect to use it in mobility, embedded systems another bandwidth-sensitive application. MQTT introduces something called “MQTT Broker”, this is in the matter of fact the heart of publish/subscribe protocol, like MQTT is.

MQTT broker is responsible for receiving messages, filtering, determining who is subscribed to the message, and sending messages to subscribed clients (devices, phones, applications…) MQTT is based on TCP/IP.

In the steps below we will go through the process of creating your Free MQTT instance using IoTaaP Console and testing it.

## IoTaaP Console account

In this step we will setup our IoTaaP Console free account for testing. Go to [IoTaaP Console](https://console.iotaap.io/auth/register) and create your account with your e-mail.

## Creating new instance

From the right sidebar select **“MQTT“**. In the MQTT page simpy click on the green "+" button, and our system will automatically generate and configure new MQTTS instance for you. 

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/iotaap-mqtt-instance.png"Create instance")


## Instance Data

If everything went well, you will see your instance up and running. You will be able to see your instance info and parameters that we will use to intereact with your instance.

**ID** is the identification string of your instance.

**Server** is the server where you can access your instance.

**Port** is the port where your instance is running.

**User*** will be used to connect with your instance, as well as the *password*.

## Testing with MQTT.fx

To test our instance, we will use **MQTT.FX**, this  is a MQTT Client written in Java based on [Eclipse Paho](http://www.eclipse.org/paho/). Download latest release of MQTT.fx from [here](https://mqttfx.jensd.de/index.php/download). Select and download MQTT for your platform (Windows, Linux…). Run your setup and install it. After running MQTT.fx, click on **Edit Connection Profiles**, cog, next to the Connect button.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/iotaap-configure-mqttfx.png"MQTT FX")

This is the place where we will add our new MQTT connection. Simply click on the **+** button in the bottom left corner and fill your new profile with the data from IoTaaP Console MQTT instance info.


**Do not forget to enter your User and Password in “User Credentials” tab!**

As our IoTaaP MQTT Broker works only with secured connection (MQTTS), we have to configure SSL certificates in order to be able to connect to the server. *Certificate is already included in our IoTaaP HAPI library, so there is no need to change it in your firmware*. 
Simply open SSL/TLS tab and provide the path to our IoTaaP MQTTS Certificate under CA Certificate file. 

**IoTaaP MQTTS Certificate**

```
-----BEGIN CERTIFICATE-----
MIIDlTCCAn2gAwIBAgIUd6oLZtAfYIjLOg2908R3hxQZ3lUwDQYJKoZIhvcNAQEL
BQAwWjELMAkGA1UEBhMCSFIxEDAOBgNVBAgMB0Nyb2F0aWExDzANBgNVBAcMBlph
Z3JlYjEPMA0GA1UECgwGSW9UYWFQMRcwFQYDVQQDDA5tcXR0LmlvdGFhcC5pbzAe
Fw0yMDAzMDgxMTA3MzhaFw0yNTAzMDcxMTA3MzhaMFoxCzAJBgNVBAYTAkhSMRAw
DgYDVQQIDAdDcm9hdGlhMQ8wDQYDVQQHDAZaYWdyZWIxDzANBgNVBAoMBklvVGFh
UDEXMBUGA1UEAwwObXF0dC5pb3RhYXAuaW8wggEiMA0GCSqGSIb3DQEBAQUAA4IB
DwAwggEKAoIBAQCtKghvPBU2xnOXsx0W/5y1ew8AYuws5cSkzukgzTkQI0XTO0n5
IT8CQD86Qi5/Q5qQtw0BQMUi5DEAPv/pHI0KuAdOaWYqlzA7AguP/4ubHvyJCNve
Id6584YGLLw3YRSHAAFuMSwUd09NcLi21CwwSnH8K6GYhSDzYC/N9E6vzlpmSb8f
48FeFkawHMrBgA/OFN7ffmIXe3QoCl4Ui3czk/rG3GN4J0rwpt16H8nvbGymIjBp
9cmya8US9lFDbpuCDaKTlH7Dla6ibMkDpOA8JnwukJEwiaEJR5wN9n3NwY/JEVXa
SXyEtQTKAqPoZj1y3CSmq/Y8FfPz1IkwPzMNAgMBAAGjUzBRMB0GA1UdDgQWBBSZ
m4E30lB6RHa1slkKEmFUMbZFWDAfBgNVHSMEGDAWgBSZm4E30lB6RHa1slkKEmFU
MbZFWDAPBgNVHRMBAf8EBTADAQH/MA0GCSqGSIb3DQEBCwUAA4IBAQB7fWkOQ1Uj
4/W4LLkFCHa+OYZIy+t3r8bgYjKoCqH8+DlXZWpwThjn8a+heTeYCknbh/D8270k
edzBhUjK06qoDWpXOI4eL3+7vffLS+AEanAi/XuiqYi7spoMSz2JwXFjc/94HpYf
WC4fcelqIdQl0qR4dnRH9otKfz4KeqhgVcjL0nyPpt2Q9UndN0nT043bthW5kk35
f2eoPel0okXFNmcvusS1pEvg8ddfM6GkJTYZr5aj5ZjitRE0ZwGBayE76JkLrSlZ
fbPnrsnBM5OA7ytgR7LyFB4bHaJCM81NAYDQGG9hcsiaZeCEOQHIR97xd32a52Tm
QySj9+LAkqon
-----END CERTIFICATE-----
```

[DOWNLOAD](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/iotaap_mqtts_certificate.pem)


Click Apply and close the widow. **Next** is to connect to our MQTT instance, select your instance from the dropdown menu, and click connect.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/mqttfx-ssl-certificate.png"CA Certificate")

In the next step we will publish our first topic!

## Publishing topic

In this step we will publish our first topic.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/mqtt-publis-topic.png"MQTT Publish new topic")

!!! warning "Topic syntax"
    You can only publish to the topics that begin with your MQTTS instance username (e.g. `/<username>/kitchen/light`). This is the root of every topic, and
    if you try to publish to a different root topic (that you do not own), MQTTS server will refuse the connection.

We have to write our topic and a message. In this example we will sent the `state` of the kitchen light as boolean parameter

``` json
{
	"state": true
}
```

Let’s say that we want to turn on our kitchen light. As you can see we will write **/tiALJgRN/kitchen/light** and this will be our topic, for example **/tiALJgRN/kitchen/fridge** could be our fridge topic.

Important things about MQTT topic names:

**Case sensitive**

**Must start with root topic `/<username>`**

**Use UTF-8 strings**

**Must consist of at least one character to be valid**

!!! warning "Payload (message body)"
    For best performance and in order to enable IoTaaP Console full debugging performance your data should be JSON structured instead of using plain text.


## Subrscribing

Now when we are sure that our connection works and that we can publish, we will subscribe our “client” to the topic.

Open your **MQTT.fx** and select **Subscribe** tab, next to the Publish tab. We will subscribe to our ‘kitchen light’ topic. Write **“/tiALJgRN/kitchen/light”** to the subscribe field and click **Subscribe**. You will see your topic listed.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/mqtt-iot-subsribe-topic.png"MQTT Subscribe")

Next, we will publish to our topic by clicking Publish button on the **Publish** tab. Click back on the **Subscribe** tab, and you will see your topic's data received. Try to change your message data and publish it again, also, you can play by adding various topics (e.g. /garden/fountain/water or /garden/fountain/light).

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/mqtt-topic-payload-published.png"MQTT Subscribe topic")