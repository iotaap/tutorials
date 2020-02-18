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
MIICojCCAgugAwIBAgIUD0bLdOOSIu4SejMW9Pb7pD9Ctz0wDQYJKoZIhvcNAQEL
BQAwYzELMAkGA1UEBhMCSFIxEDAOBgNVBAgMB0Nyb2F0aWExDzANBgNVBAcMBlph
Z3JlYjEPMA0GA1UECgwGSW9UYWFQMSAwHgYJKoZIhvcNAQkBFhFjb250YWN0QGlv
dGFhcC5pbzAeFw0yMDAyMTgxNDM2MDRaFw0yNTAyMTcxNDM2MDRaMGMxCzAJBgNV
BAYTAkhSMRAwDgYDVQQIDAdDcm9hdGlhMQ8wDQYDVQQHDAZaYWdyZWIxDzANBgNV
BAoMBklvVGFhUDEgMB4GCSqGSIb3DQEJARYRY29udGFjdEBpb3RhYXAuaW8wgZ8w
DQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAOf10hAizEoCvH7EokaLU7bIvcCbiEK0
2p5LEFdaO/iwT8PeWe5x0guZNSLE8ttJs7GGAiPUPtGJ2obVANEB11zT14yxcQ1Z
X8tGUcgWrveBSslfNa9ogY0q5a/7HD2GuWc8gtXP5B8gs+xV/byI92syJdj3Ogfr
jpDaBXxnxU4LAgMBAAGjUzBRMB0GA1UdDgQWBBR9PqwhE3qfvaVoOYozlLn5XzSK
YzAfBgNVHSMEGDAWgBR9PqwhE3qfvaVoOYozlLn5XzSKYzAPBgNVHRMBAf8EBTAD
AQH/MA0GCSqGSIb3DQEBCwUAA4GBAH+kXaiJ8ZhAAeBsExuJyzO/tOUIel2gGaO3
GJV+RGPTmKAi8py4GNMQUSTPoFRIjBFMttzoeZrSNrVuyj0h7IXjxQ0fOf3JwCvQ
eLJDXrz7Gt97yVi8YcPMDzCnPZxAMHe+DBHUtHXNtzQNn2kA37IxY3NlViLU8Mf6
/AcmCDWO
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