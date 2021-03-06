# IoTaaP - MQTT

In this tutorial you will learn how to connect to MQTT broker using IoTaaP and we will create a simple program that will publish accelerometer coordinates and listen for LED status commands.

## Setup

We must define our features before _#include <IoTaaP.h>_

```cpp
#define ENABLE_JSON
```
In _**setup()**_ function we wil configure WiFi and connect IoTaaP to WiFi AP (with internet access)

```cpp
iotaap.wifi.connect("<your-ssid>","<your-password>");
```
Now it’s time to initialize MQTT object and connect IoTaaP to our IoTaaP MQTT instance.

```cpp
iotaap.mqtt.connect("iotaap_client", "<some-mqtt-broker>", 8883, callback, false, "USERNAME", "PASSWORD");
```

!!! warning "IoTaaP Cloud supports only MQTTS"
    Please note that from IoTaaP 2.0 release in January 2021, in order to enhance security of our system, IoTaaP Cloud supports only **MQTTS** connection and strictly defined **root topic** rules (every topic
    starts with your MQTTS username, e.g. `/<your-username>/<your-topic>`).

This line of code will connect IoTaaP to the IoTaaP MQTT broker. You can get connection credentials in your instance details in IoTaaP Console.

**callback()** is the function that will be called every time when something arrives to our subscribed topic(s). We will define this function later.

```cpp
iotaap.mqtt.subscribe("/<your-username>/ledstatus/led1");
```
This line of code will subscribe IoTaaP to `/<your-username>/ledstatus/led1` topic. We will define our callback in the next step.

## Callback function

Callback function signature is already defined so it cannot be changed and it looks like this:

```cpp
oid callback(char* topic, byte* message, unsigned int length) 
```
Now we will define the function body:

```cpp
void callback(char* topic, byte* message, unsigned int length) {
  String messageTemp;
  
  for (int i = 0; i < length; i++) {
    messageTemp += (char)message[i];
  }

  if (String(topic) == "/<your-username>/ledstatus/led1") {
    if(messageTemp == "on"){
      iotaap.misc.setPin(ONBOARD_LED1);
    }
    else if(messageTemp == "off"){
      iotaap.misc.clearPin(ONBOARD_LED1);
    }
  }
}
```
This function will convert our message into **String** type and it will check if our trigger topic was `/<your-username>/ledstatus/led1`, if this is true, our function will check message content. Content must be “on” in order to turn the LED1 on, or “off” to turn it off.

## Loop

!!! info "IoTaaP Magnolia V2 development board"
    Please note that our production version of IoTaaP Magnolia Development Board does not contain accelerometer anymore, and accelerometer option is removed
    from the IoTaaP Core library. But this example is a good starting point for your own implementation.

In our main loop we will publish our accelerometer data in jSON format, and we will call mandatory function _keepAlive()_ that will keep our MQTT connection active.

IoTaaP library comes with ArduinoJson (C++ JSON library for Arduino and IoT) for easier handling of the jSON format. We will use only _serializeJson()_ function in this example, but feel free to check other features of ArduinoJson library.

```cpp
void loop()
{
  iotaap.mqtt.keepAlive();

  doc["x"] = 510;
  doc["y"] = 10;
  doc["z"] = 24;

  char accJson[255];

  serializeJson(doc, accJson);

  iotaap.mqtt.publish("/<your-username>/iotaap/accelerometer", accJson, false);

  delay(200);
}
```
This code will keep our MQTT connection active, but it will also read accelerometer data (in accelerometer structure). Axis values will be saved to “doc” with appropriate axis names. Function _serializeJson(doc,accJson);_ will convert **doc** to jSON string, and finally _iotaap.mqtt.publish(`/<your-username>/iotaap/accelerometer`, accJson);_ will publish values to `/<your-username>/iotaap/accelerometer` topic.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/iotaap-mqtt/iotaap-accelerometer-topic-1024x368.jpg"IoTaaP/accelerometer")

## Testing

We will use MQTT.fx for testing. MQTT.fx installation process and usage are described in **MQTT Setup**. Simply connect to your IoTaaP MQTT instance and publish on or off to `/<your-username>/ledstatus/led1` topic. Be sure to subscribe to the `/<your-username>/iotaap/accelerometer` topic and you will se accelerometer coordinates in JSON format

```json
{
  "x" : 510,
  "y" : 10,
  "z" : 24
}
```

Now, create a code that will control 2 LED’s and publish button status, share your solutions with the [**community**](https://community.iotaap.io/).
