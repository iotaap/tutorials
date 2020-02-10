# IoTaaP - API

IoTaaP implements MQTT API which allows to read and write data from/to IoTaaP hardware modules using IoTaaP Cloud. In this guide we will create a simple project that will use IoTaaP basic API.

## API Read

IoTaaP API supports bi-directional communication. Read API is used to get data from the device.

Every IoTaaP module with enabled API (we will do this programmatically in the next steps) will publish its state to the specific cloud topic. Our IoTaaP Cloud and its microservices will process this request and either transfer data to the listener topic(s) (e.g. to some application channel) or transfer data to the database or any other internal service. In this guide, we will focus on the listener topic.

If API is enabled and device is properly connected to the **IoTaaP Cloud** device state will be available on the following topic:

```
api/listen/<device_id>
```
Where _**<device_id>**_ is the predefined unique device ID defined programmatically. Data will be pushed in the JSON format:

```json
{
  "onboard" : {
    "but1" : false,
    "but2" : false
  },
  "digital" : {
    "2" : false,
    "4" : false,
    "12" : false,
    "13" : false,
    "14" : false,
    "15" : false,
    "16" : false,
    "17" : false,
    "26" : false,
    "27" : false,
    "32" : false,
    "33" : false,
    "34" : false,
    "35" : false
  },
  "analog" : {
    "16" : 0,
    "17" : 0,
    "32" : 210,
    "33" : 1819,
    "34" : 1840,
    "35" : 1040
  }
}
```
JSON will contain a few groups:

**onboard – onboard inputs – Button states**

**digital – digital input states**

**analog – analog input states**

Number indicates the real pin number and values will depend on the configuration.

Onboard group values can be only **true** and **false**, as well as digital group values. Analog values are 16 bit.

Refresh interval can be configured programmatically.

## API Write

IoTaaP Write API is used in order to control IoTaaP outputs and other features (e.g. Serial output) in more convinient way and remotely (e.g. from some web or mobile application). IoTaaP API only works with the **IoTaaP Cloud** (please feel free to contact us if you have any questions regarding IoTaaP Cloud, or  [** ask the community**](https://community.iotaap.io/)).

You can control your device by publishing predefined JSON to the following topic:

```
api/transfer
```
Microservices on the IoTaaP cloud will process the data and safely deliver it to the device.

Here are the examples of the JSON commands:

```json
{
  "device": "light_device",
  "data": {
    "output": {
      "pin": 14,
      "type": "digital",
      "value": true
    }
  }
}
```
This snippet will set the pin 14 to true (HIGH) on the device(s) named “**light_device**“. You must keep the same structure of your JSON snippet.

**device – Device name String (at least 1 character)**

**pin – pin to write to (any available pin on the IoTaaP Module)**

**type – pin type (analog or digital)**

**value – pin value (true/false for digital type and 0-65535 for analog type)**

```json
{
  "device": "light_device",
  "data": {
    "output": {
      "pin": 14,
      "type": "analog",
      "value": 32768
    }
  }
}
```
This snippet will set pin 14 to the analog value of 32768 (approximately 50% )

```json
{
  "device": "light_device",
  "data": {
    "serial": {
      "data": "Hello world"
    }
  }
}
```
This snippet will send “Hello world” message to the IoTaaP serial port.

All pins or functions (like Serial) will be automatically configured in the firmware and all data will be prepared on the IoTaaP Cloud.

In the next steps you will see how easy you can enable IoTaaP API in your firmware using [**IoTaaP library**](https://platformio.org/lib/show/6697/IoTaaP).

## Coding

We will write a simple code that will publish IoTaaP state every 300ms and listen for incoming commands through API. This example is based on the [MQTTS example](https://www.iotaap.io/instructions/iotaap-mqtts/).

First we will define constants:

```cpp
#define DEVICE_ID "fi3v849t4973v"
#define CONTROL_TOPIC "light_device"
```
DEVICE_ID will be used for publishing the device state and CONTROL_TOPIC will be used for “receiving” commands.

Next, in the MQTT(S) callback function we must add our _callbackInnerFunction_ from the IoTaaP library. This function will handle everything connected with the IoTaaP API.

```cpp
void callback(char *topic, byte *message, unsigned int length)
{
  iotaap.mqtt.callbackInnerFunction(CONTROL_TOPIC,topic, message, length);
}
```
In the _setup()_ we must connect our module to the cloud and set DEVICE_ID as MQTT client ID:

```cpp
 iotaap.mqtt.connect(DEVICE_ID, "iotaap.cloud", 8883, callback, true, "<username>", "<password>", CA_cert);
  iotaap.mqtt.enableApi();
```
Also, as you can see, we must call _enableApi()_ function that will basically subscribe our module to the _“api/#”_ topics. Function will also initialize serial port with the specified or default baud rate (115200).

Finally, if we want to publish device state to the cloud, we must call _apiLoop()_ function in the _loop()_. If you only want to use IoTaaP incomming API, you have to call _keepAlive()_ function in the _loop()_.

```cpp
void loop()
{
  //iotaap.mqtt.keepAlive();
  iotaap.mqtt.apiLoop(300);
}
```
_apiLoop()_ by default has 10ms interval, but this interval can be changed by passing the parameter in the function. _apiLoop()_ implements the _keepAlive()_ function, so it is not needed if you are using _apiLoop()_.

## Final code

This final code can also be found under **examples** in the IoTaaP library.

```cpp
#define ENABLE_JSON

#include "IoTaaP.h"

#define DEVICE_ID "hdf7h74hufwhu"
#define CONTROL_TOPIC "my_device"

IoTaaP iotaap; // IoTaaP library

// Let's Encrypt Cross Signed certificate
// More info about Let's Encrypt Chain of Trust can be found here: https://letsencrypt.org/certificates/
// We are using this certificate: https://letsencrypt.org/certs/trustid-x3-root.pem.txt
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

void callback(char *topic, byte *message, unsigned int length)
{
  iotaap.mqtt.callbackInnerFunction(CONTROL_TOPIC,topic, message, length);
}

void setup()
{
  iotaap.wifi.connect("<ssid>", "<password>");

  iotaap.mqtt.connect(DEVICE_ID, "iotaap.cloud", 8883, callback, true, "<username>", "<password>", CA_cert);
  iotaap.mqtt.enableApi();

  delay(500);
}

void loop()
{
  //iotaap.mqtt.keepAlive();
  iotaap.mqtt.apiLoop(300);
}
```