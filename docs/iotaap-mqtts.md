# IoTaaP - MQTTS

This example is pretty similar to the [IoTaaP – MQTT](https://www.iotaap.io/instructions/iotaap-mqtt/). But pure MQTT communication is not safe, that’s why we prefere to use MQTTS (MQTT + SSL). In this example we will use Let’s Encrypt’s Cross Signed certificate, **IoTaaP Cloud** MQTT broker instance (although everything should work with other MQTT brokers).

# Setup

We must define our features before _#include <IoTaaP.h>_

```cpp
#define ENABLE_JSON
```
In _**setup()**_ function we wil configure WiFi and connect IoTaaP to WiFi AP (with internet access)

```cpp
iotaap.wifi.connect("<your-ssid>","<your-password>");
```
Now it’s time to initialize MQTT object and connect IoTaaP to our **iotaap.cloud** instance.

```cpp
iotaap.mqtt.connect("iotaap_client", "iotaap.cloud", 8883, callback, true, "USERNAME", "PASSWORD", CA_cert);
```
This line of code will connect IoTaaP to the Mosquitto broker running on IoTaaP Cloud. Notice the new parameter here: **CA_cert**. In the [IoTaaP – MQTT](https://www.iotaap.io/instructions/iotaap-mqtt/) example **secure** parameter was set to **false**, now it’s set to **true** and we have to pass CA Certificate to the function.

We are using Let’s Encrypt Cross Signed certificate for this, if your MQTT broker provider is using different Certificate provider for their server instance, you will probably need to obtain the CA Certificate or Root certificate of their Certificate provider. You can read more about Let’s Encrypt Chain of Trust [here](https://letsencrypt.org/certificates/).

In order to enhance our security, we are using Cross Signed certificate. Let’s Encrypt Cross signed certificate can be found [here](https://letsencrypt.org/certs/trustid-x3-root.pem.txt).

In order to user CA Certificate you will have to define it just below **IoTaaP iotaap;** line in your code:

```cpp
const char *CA_cert = "-----BEGIN CERTIFICATE-----\n"
                      "MIIDSjCCAjKgAwIBAgIQRK+wgNajJ7qJMDmGLvhAazANBgkqhkiG9w0BAQUFADA/\n"
                      "MSQwIgYDVQQKExtEaWdpdGFsIFNpZ25hdHVyZSBUcnVzdCBDby4xFzAVBgNVBAMT\n"
                      "DkRTVCBSb290IENBIFgzMB4XDTAwMDkzMDIxMTIxOVoXDTIxMDkzMDE0MDExNVow\n"
                      "PzEkMCIGA1UEChMbRGlnaXRhbCBTaWduYXR1cmUgVHJ1c3QgQ28uMRcwFQYDVQQD\n"
                      "Ew5EU1QgUm9vdCBDQSBYMzCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEB\n"
                      "AN+v6ZdQCINXtMxiZfaQguzH0yxrMMpb7NnDfcdAwRgUi+DoM3ZJKuM/IUmTrE4O\n"
                      "rz5Iy2Xu/NMhD2XSKtkyj4zl93ewEnu1lcCJo6m67XMuegwGMoOifooUMM0RoOEq\n"
                      "OLl5CjH9UL2AZd+3UWODyOKIYepLYYHsUmu5ouJLGiifSKOeDNoJjj4XLh7dIN9b\n"
                      "xiqKqy69cK3FCxolkHRyxXtqqzTWMIn/5WgTe1QLyNau7Fqckh49ZLOMxt+/yUFw\n"
                      "7BZy1SbsOFU5Q9D8/RhcQPGX69Wam40dutolucbY38EVAjqr2m7xPi71XAicPNaD\n"
                      "aeQQmxkqtilX4+U9m5/wAl0CAwEAAaNCMEAwDwYDVR0TAQH/BAUwAwEB/zAOBgNV\n"
                      "HQ8BAf8EBAMCAQYwHQYDVR0OBBYEFMSnsaR7LHH62+FLkHX/xBVghYkQMA0GCSqG\n"
                      "SIb3DQEBBQUAA4IBAQCjGiybFwBcqR7uKGY3Or+Dxz9LwwmglSBd49lZRNI+DT69\n"
                      "ikugdB/OEIKcdBodfpga3csTS7MgROSR6cz8faXbauX+5v3gTt23ADq1cEmv8uXr\n"
                      "AvHRAosZy5Q6XkjEGB5YGV8eAlrwDPGxrancWYaLbumR9YbK+rlmM6pZW87ipxZz\n"
                      "R8srzJmwN0jP41ZL9c8PDHIyh8bwRLtTcm1D9SZImlJnt1ir/md2cXjbDaJWFBM5\n"
                      "JDGFoqgCWjBH4d1QB7wCCZAA62RjYJsWvIjJEubSfZGL+T0yjWW06XyxV3bqxbYo\n"
                      "Ob8VZRzI9neWagqNdwvYkQsEjgfbKbYK7p2CNTUQ\n"
                      "-----END CERTIFICATE-----\n";
```
**Notice that we are using ‘\n’ and this is the only way to make your certificate work properly, so if you are using different certificate make sure to keep the same formatting.** 

**callback()** is the function that will be called every time when something arrives to our subscribed topic(s). We will define this function later.

```cpp
iotaap.mqtt.subscribe("ledstatus/led1");
```
This line of code will subscribe IoTaaP to “ledstatus/led1” topic. We will define our callback in the next step.

_Keep in mind that every CA Certificate or Root certificate has it’s own expiration date. If the certificate is not valid or if it’s expired your MQTTS connection will not work! Make sure to implement some check-update mechanism to update your certificates stored on your IoTaaP magnolia board. The best way to implement this mechanism is to use OTA or FOTA. With OTA or FOTA you will be able to update your device’s firmware remotely with the new version that contains updated certificate._

## Callback function

Callback function signature is already defined so it cannot be changed and it looks like this:

```cpp
void callback(char* topic, byte* message, unsigned int length) 
```
Now we will define the function body:

```cpp
void callback(char* topic, byte* message, unsigned int length) {
  String messageTemp;
  
  for (int i = 0; i < length; i++) {
    messageTemp += (char)message[i];
  }

  if (String(topic) == "ledstatus/led1") {
    if(messageTemp == "on"){
      iotaap.misc.setPin(ONBOARD_LED1);
    }
    else if(messageTemp == "off"){
      iotaap.misc.clearPin(ONBOARD_LED1);
    }
  }
}
```
This function will convert our message into **String** type and it will check if our trigger topic was _“ledstatus/led1“_, if this is true, our function will check message content. Content must be “on” in order to turn the LED1 on, or “off” to turn it off.

## Loop

In our main loop we will publish our accelerometer data in jSON format, and we will call mandatory function keepAlive() that will keep our MQTT connection active.

IoTaaP library comes with ArduinoJson (C++ JSON library for Arduino and IoT) for easier handling of the jSON format. We will use only serializeJson() function in this example, but feel free to check other features of ArduinoJson library.

```cpp
void loop()
{
  iotaap.mqtt.keepAlive();

  accelerometer acc = iotaap.accelerometer.getRaw();

  doc["x"] = acc.x;
  doc["y"] = acc.y;
  doc["z"] = acc.z;

  char accJson[255];

  serializeJson(doc, accJson);

  iotaap.mqtt.publish("iotaap/accelerometer", accJson);

  delay(200);
}
```
This code will keep our MQTT connection active, but it will also read accelerometer data (in accelerometer structure). Axis values will be saved to “doc” with appropriate axis names. Function serializeJson(doc,accJson); will convert **doc** to jSON string, and finally _iotaap.mqtt.publish(“iotaap/accelerometer”, accJson);_ will publish values to _“iotaap/accelerometer”_ topic.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/iotaap-mqtt/iotaap-accelerometer-topic-1024x368.jpg"IoTaaP/accelerometer")

## Testing

We will use MQTT.fx for testing. MQTT.fx installation process and usage are described [here](https://www.iotaap.io/instructions/cloudmqtt-setup/#1568209144227-ca19357a-d1f5). Simply connect to your **IoTaaP Cloud** (or any other MQTT broker) instance and publish on or off to “ledstatus/led1” topic. Be sure to subscribe to the “iotaap/accelerometer” topic and you will se accelerometer coordinates in jSON format

```json
{
  "x" : 1921,
  "y" : 1936,
  "z" : 1109
}
```
Now, create a code that will control 2 LED’s and publish button status, share your solutions with the [community](https://community.iotaap.io/).