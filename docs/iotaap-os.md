# IoTaaP OS

The unique system built for ESP32 SoC which provides everything you need to successfully develop, deploy and manage your IoT devices.

## System Modules

* WiFi Module - Manages WiFi connection to the AP
* MQTT Module - Manages MQTTS connection with IoTaaP Cloud
* OTA Update Module - Periodically checking for updates and manages remote flashing process
* HMI Module - Managing LED and Serial/MQTT configuration menu
* File System Module - Managing file system on SD card and internal FAT system
* System Process Module - Manages various system processes for normal operation

## IoTaaP OS Documentation

In order to easily deploy and explore the following example, please first read the [IoTaaP OS Documentation](https://docs.iotaap.io/docs-iotaap-os/). Internal filesystem will be automatically created during first boot. 
Since version 4 of the IoTaaP OS, SD card is not mandatory in order for system to boot. If SD card is not present features like data backup and logs will be disabled. Since version 4, IoTaaP OS uses Web Configurator
for configuration. 

## Installing IoTaaP OS using PlatformIO

In order to start using IoTaaP OS, we recommend using PlatformIO framework, info on how to start a new project using PlatformIO, can be found [here](https://docs.iotaap.io/docs-tutorials/working-environment/).

You have to declare library dependencies in your `platformio.ini` file, by using [lib_deps](https://docs.platformio.org/page/projectconf/section_env_library.html) option. You can define the specific library version, although we recommend using the newest version in your projects.

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

  iotaapOs.start(); // Start IoTaaP OS

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

## Example PlatformIO project configuration

```
[env:myenv]
platform = espressif32
board = iotaap_magnolia
framework = arduino

lib_deps =

     # RECOMMENDED
     # Accept new functionality in a backwards compatible manner and patches
     iotaap/IoTaaP OS @ ^4.0.0

     # Accept only backwards compatible bug fixes
     # (any version with the same major and minor versions, and an equal or greater patch version)
     iotaap/IoTaaP OS @ ~4.0.0

     # The exact version
     iotaap/IoTaaP OS @ 4.0.0

```

More info about Library installation using PlatformIO can be found [here](https://platformio.org/lib/show/11733/IoTaaP/installation).

## Using custom partitions with ESP32

Although IoTaaP OS can work with 4MB of flash memory, we recommend using ESP32 modules with 16MB of flash. Starting from version 4.0.0, IoTaaP OS
requires custom partition table in order to work. Configuration .csv files are available under **partitions** directory of the library. Custom partition
files can be configured in `platformio.ini`:

```
board_build.partitions = partitions/default_16MB_fat.csv
board_upload.flash_size=16MB

```

Please note that `partitions` directory should be on the same level as your `src` directory, for the above configuration to work properly.

## Known issues

!!! warning "Known issue with ESP32 framework"
    Please note that we are aware of the issue with *arduino-esp32* framework, caused by **WiFiClientSecure** library. You can read more
    about this issue, and find the **workaround** [here](https://community.iotaap.io/t/esp32-restarts/)