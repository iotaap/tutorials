# IoTaaP OS

The unique system built for ESP32 SoC which provides everything you need to successfully develop, deploy and manage your IoT devices.

## System Modules

* WiFi Module - Manages WiFi connection to the AP
* MQTT Module - Manages MQTTS connection with IoTaaP Cloud
* OTA Update Module - Periodically checking for updates and manages update process
* HMI Module - Managing LED and Serial/MQTT configuration menu
* File System Module - Managing file system on SD card
* System Process Module - Manages various system processes for normal operation

## IoTaaP OS Documentation

In order to easily deploy and explore the following example, please first read the [IoTaaP OS Documentation](https://docs.iotaap.io/docs-iotaap-os/). Prepare
your SD card, [download FS Structure](https://files.iotaap.io/assets/iotaap-os/assets/fs_structure.zip) and extract it to your SD card. Don't forget to update *default.cfg* with your data, obtained from [IoTaaP Console](https://console.iotaap.io).

## Example

In the following example you can see how to use **IoTaaP OS** in order to successfully connect your device to the cloud,
and manage it using [IoTaaP Console](https://console.iotaap.io)

``` cpp
#include <IoTaaP_OS.h>

IoTaaP_OS iotaapOs("1.0.1");

void callback(char *topic, byte *message, unsigned int length)
{
  Serial.println("---------------------------");
  Serial.println("Received data on topic:");
  Serial.println(topic); // Print topic

  Serial.println("Data:");

  for (int i = 0; i < length; i++) // Print message
  {
    Serial.print((char)message[i]);
  }
  Serial.println();
  Serial.println("---------------------------");
}

void setup()
{
  char ownerString[128]; // Char array used to store custom parameter
  char deviceId[30];     // Char array used to store system parameter

  iotaapOs.startWifi(); // Connect to WiFi
  iotaapOs.startMqtt(callback); // Connect to MQTT broker

  iotaapOs.writeToSystemLogs("Device started"); // Write data to system log using 'USER' tag

  iotaapOs.getUserParameter("owner", ownerString); // Get 'owner' parameter from 'custom.cfg'
  Serial.println("owner parameter:");
  Serial.println(ownerString);

  iotaapOs.getSystemParameter("device_id", deviceId); // Get 'device_id' parameter from 'default.cfg'
  Serial.println("device_id parameter:");
  Serial.println(deviceId);

  iotaapOs.deviceCloudPublish("{\"my_text\":\"Hello IoT World!\"}", "hello_topic");        // Publish simple (escaped) JSON to: /<username>/devices/<device-id>/hello_topic
  iotaapOs.basicCloudPublish("{\"my_simple_text\":\"Hello IoT World!\"}", "simple_topic"); // Publish simple (escaped) JSON to: /<username>/simple_topic

  iotaapOs.basicSubscribe("dummy_topic"); // Subscribe to /<username>/dummy_topic

  iotaapOs.basicSubscribe("receiving_topic"); // Subscribe to /<username>/receiving_topic
  // Every message received on this topic will trigger callback

  iotaapOs.basicUnsubscribe("dummy_topic"); // Unsubscribe from /<username>/dummy_topic

  iotaapOs.checkForUpdates(); // Manually check for updates at startup

  delay(1000);
}

void loop()
{
  iotaapOs.deviceCloudPublishParam("temp", random(0, 500) / 10.0); // Publish parameter (to topic: /<username>/devices/<device-id>/params)
  iotaapOs.deviceCloudPublishParam("humi", random(0, 1000) / 10.0); // Publish parameter (to topic: /<username>/devices/<device-id>/params)
  iotaapOs.deviceCloudPublishParam("pres", random(0, 15000) / 10.0); // Publish parameter (to topic: /<username>/devices/<device-id>/params)
  delay(1000);
}

```