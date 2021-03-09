# IoTaaP Cloud - MQTTS

IoTaaP Cloud gives you everything you need to successfuly manage, update and secure your IoT devices and services. Our **IoTaaP Cloud Console** gives you the possibility
to add new devices, make OTA Updates, manage MQTTS instances, develop your IoT solution and finally bring your IoT solution online. 

!!! info "IoTaaP OS"
    Managing your edge device security, maintaining stable connection and doing remote upgrades can be a tough thing to do. Because of the same struggle, we have created
    our **IoTaaP OS**. Unique Open Source solution built on top of FreeRTOS for ESP32 SoC which provides everything you need to successfully develop, deploy and manage your IoT devices. [Check out IoTaaP OS!](https://docs.iotaap.io/docs-iotaap-os/)

## IoTaaP Console account

In this step we will setup our IoTaaP Console free account for testing. Go to [IoTaaP Console](https://console.iotaap.io/auth/register) and create your account with your e-mail.

### Creating devices and MQTT instances

Video tutorial:

[![IoTaaP Instances](http://img.youtube.com/vi/zupJiFhtNBg/0.jpg)](http://www.youtube.com/watch?v=zupJiFhtNBg "IoTaaP Instances")

## IoTaaP MQTT broker

One of the most crucial IoT parts is the communication channel. One of the most common protocols used for IoT communication is MQTT (Message Queuing Telemetry Transport) which provides lightweight methods of carrying out messaging using publish and subscribe queueing model.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/publisher-subscriber.jpg"MQTT protocol")

MQTT is ideal for IoT, it is designed to be minimal and this is perfect to use it in mobility, embedded systems another bandwidth-sensitive application. MQTT introduces something called “MQTT Broker”, this is in the matter of fact the heart of publish/subscribe protocol, like MQTT is.

MQTT broker is responsible for receiving messages, filtering, determining who is subscribed to the message, and sending messages to subscribed clients (devices, phones, applications…) MQTT is based on TCP/IP.

In the steps below we will go through the process of creating your Free MQTT instance using IoTaaP Console and testing it.

## Creating new instance

From the right sidebar select **“MQTT“**. In the MQTT page simply click on the green "+" button, and our system will automatically generate and configure new MQTTS instance for you. 

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/iotaap-mqtt-instance.png"Create instance")

!!! info "MQTT Pool"
    Please note that from IoTaaP 2.0 release in January 2021, our system uses pool of MQTT instances. Maximum number of MQTT instances per account is 8, but number of concurrent connections depends on the payment model selected. For more info, check our [Pricing](https://www.iotaap.io/pricing/). 


## Instance Data

If everything went well, you will see your instance up and running. You will be able to see your instance info and parameters that we will use to intereact with your instance.

- **User** will be used to connect with your instance
- **Password** will be used to connect with your instance
- **Server** is the server where you can access your instance
- **Port** is the port where your instance is running

## Testing with MQTT.fx

To test our instance, we will use **MQTT.FX**, this  is a MQTT Client written in Java based on [Eclipse Paho](http://www.eclipse.org/paho/). Download latest release of MQTT.fx from [here](https://mqttfx.jensd.de/index.php/download). Select and download MQTT for your platform (Windows, Linux…). Run your setup and install it. After running MQTT.fx, click on **Edit Connection Profiles**, cog, next to the Connect button.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/iotaap-configure-mqttfx.png"MQTT FX")

This is the place where we will add our new MQTT connection. Simply click on the **+** button in the bottom left corner and fill your new profile with the data from IoTaaP Console MQTT instance info.


**Do not forget to enter your User and Password in “User Credentials” tab!**

!!! info "MQTTS Certificate"
    As our IoTaaP MQTT Broker works only with secured connection (MQTTS), we have to configure SSL certificates in order to be able to connect to the server. 
    Simply open SSL/TLS tab and provide the path to our IoTaaP MQTTS Certificate under CA Certificate file. 

**IoTaaP MQTTS Certificate**

Our system is fully protected by Sectigo.

![alt text](https://files.iotaap.io/assets/iotaap-os/assets/sectigo_seal.png )

**Root Certificate**

```
-----BEGIN CERTIFICATE-----
MIIGEzCCA/ugAwIBAgIQfVtRJrR2uhHbdBYLvFMNpzANBgkqhkiG9w0BAQwFADCB
iDELMAkGA1UEBhMCVVMxEzARBgNVBAgTCk5ldyBKZXJzZXkxFDASBgNVBAcTC0pl
cnNleSBDaXR5MR4wHAYDVQQKExVUaGUgVVNFUlRSVVNUIE5ldHdvcmsxLjAsBgNV
BAMTJVVTRVJUcnVzdCBSU0EgQ2VydGlmaWNhdGlvbiBBdXRob3JpdHkwHhcNMTgx
MTAyMDAwMDAwWhcNMzAxMjMxMjM1OTU5WjCBjzELMAkGA1UEBhMCR0IxGzAZBgNV
BAgTEkdyZWF0ZXIgTWFuY2hlc3RlcjEQMA4GA1UEBxMHU2FsZm9yZDEYMBYGA1UE
ChMPU2VjdGlnbyBMaW1pdGVkMTcwNQYDVQQDEy5TZWN0aWdvIFJTQSBEb21haW4g
VmFsaWRhdGlvbiBTZWN1cmUgU2VydmVyIENBMIIBIjANBgkqhkiG9w0BAQEFAAOC
AQ8AMIIBCgKCAQEA1nMz1tc8INAA0hdFuNY+B6I/x0HuMjDJsGz99J/LEpgPLT+N
TQEMgg8Xf2Iu6bhIefsWg06t1zIlk7cHv7lQP6lMw0Aq6Tn/2YHKHxYyQdqAJrkj
eocgHuP/IJo8lURvh3UGkEC0MpMWCRAIIz7S3YcPb11RFGoKacVPAXJpz9OTTG0E
oKMbgn6xmrntxZ7FN3ifmgg0+1YuWMQJDgZkW7w33PGfKGioVrCSo1yfu4iYCBsk
Haswha6vsC6eep3BwEIc4gLw6uBK0u+QDrTBQBbwb4VCSmT3pDCg/r8uoydajotY
uK3DGReEY+1vVv2Dy2A0xHS+5p3b4eTlygxfFQIDAQABo4IBbjCCAWowHwYDVR0j
BBgwFoAUU3m/WqorSs9UgOHYm8Cd8rIDZsswHQYDVR0OBBYEFI2MXsRUrYrhd+mb
+ZsF4bgBjWHhMA4GA1UdDwEB/wQEAwIBhjASBgNVHRMBAf8ECDAGAQH/AgEAMB0G
A1UdJQQWMBQGCCsGAQUFBwMBBggrBgEFBQcDAjAbBgNVHSAEFDASMAYGBFUdIAAw
CAYGZ4EMAQIBMFAGA1UdHwRJMEcwRaBDoEGGP2h0dHA6Ly9jcmwudXNlcnRydXN0
LmNvbS9VU0VSVHJ1c3RSU0FDZXJ0aWZpY2F0aW9uQXV0aG9yaXR5LmNybDB2Bggr
BgEFBQcBAQRqMGgwPwYIKwYBBQUHMAKGM2h0dHA6Ly9jcnQudXNlcnRydXN0LmNv
bS9VU0VSVHJ1c3RSU0FBZGRUcnVzdENBLmNydDAlBggrBgEFBQcwAYYZaHR0cDov
L29jc3AudXNlcnRydXN0LmNvbTANBgkqhkiG9w0BAQwFAAOCAgEAMr9hvQ5Iw0/H
ukdN+Jx4GQHcEx2Ab/zDcLRSmjEzmldS+zGea6TvVKqJjUAXaPgREHzSyrHxVYbH
7rM2kYb2OVG/Rr8PoLq0935JxCo2F57kaDl6r5ROVm+yezu/Coa9zcV3HAO4OLGi
H19+24rcRki2aArPsrW04jTkZ6k4Zgle0rj8nSg6F0AnwnJOKf0hPHzPE/uWLMUx
RP0T7dWbqWlod3zu4f+k+TY4CFM5ooQ0nBnzvg6s1SQ36yOoeNDT5++SR2RiOSLv
xvcRviKFxmZEJCaOEDKNyJOuB56DPi/Z+fVGjmO+wea03KbNIaiGCpXZLoUmGv38
sbZXQm2V0TP2ORQGgkE49Y9Y3IBbpNV9lXj9p5v//cWoaasm56ekBYdbqbe4oyAL
l6lFhd2zi+WJN44pDfwGF/Y4QA5C5BIG+3vzxhFoYt/jmPQT2BVPi7Fp2RBgvGQq
6jG35LWjOhSbJuMLe/0CjraZwTiXWTb2qHSihrZe68Zk6s+go/lunrotEbaGmAhY
LcmsJWTyXnW0OMGuf1pGg+pRyrbxmRE1a6Vqe8YAsOf4vmSyrcjC8azjUeqkk+B5
yOGBQMkKW+ESPMFgKuOXwIlCypTPRpgSabuY0MLTDXJLR27lk8QyKGOHQ+SwMj4K
00u/I5sUKUErmgQfky3xxzlIPK1aEn8=
-----END CERTIFICATE-----
```

[DOWNLOAD](https://files.iotaap.io/assets/iotaap-os/assets/ca.crt)

!!! warning "Invalid certificate"
    System will not be able to connect to the MQTT server or do the OTA update if certificate is missing or invalid.

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


## Subscribing

Now when we are sure that our connection works and that we can publish, we will subscribe our “client” to the topic.

Open your **MQTT.fx** and select **Subscribe** tab, next to the Publish tab. We will subscribe to our ‘kitchen light’ topic. Write **“/tiALJgRN/kitchen/light”** to the subscribe field and click **Subscribe**. You will see your topic listed.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/mqtt-iot-subsribe-topic.png"MQTT Subscribe")

Next, we will publish to our topic by clicking Publish button on the **Publish** tab. Click back on the **Subscribe** tab, and you will see your topic's data received. Try to change your message data and publish it again, also, you can play by adding various topics (e.g. /garden/fountain/water or /garden/fountain/light).

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/cloudmqtt-setup/mqtt-topic-payload-published.png"MQTT Subscribe topic")