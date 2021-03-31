# IoTaaP OS Parameters.

## What is a parameter?

A parameter is a value of a specific type used within a system to convey information.

Within IoTaaP OS we divide parameters into two categories, system and user.

IoTaaP OS uses the file system on the SD card to store many of these parameters. Specifically under the etc/device/ directory. Within the default.cfg and custom.cfg files.


## System Parameters

These are critical system informations. Often unique to the device.
A collection of these are read from the SD card file structure by the system. This is where the default.cfg file holds the system parameters.

From this .cfg file the system can read the JSON structure allowing us to call and use these parameters.

To call a system parameter we use the function.

```iotaapOs.getSystemParameter("device_id", deviceId);```

We can use this statement to get any of the parameters from the default.cfg file and it will return the second half of the key pair. (Unfamilar? [Re-read JSON tutorial](iotaap-os-mqtts-basics-JSON.md))

Notice we need to save this value locally as a Char array. Although this method gets the parameter we still need to store it.
A full list of the system parameters can be found under the [config-file docs](https://docs.iotaap.io/docs-iotaap-os/config-file/).

```cpp
#include <IoTaaP_OS.h>

IoTaaP_OS iotaapOs("1.0.1");

void setup()
{
  char deviceId[30]; // Char array used to store system parameter
  int timezone[10]; // Int value holding timezone system parameter   

  iotaapOs.getSystemParameter("device_id", deviceId); // Get 'device_id' parameter from 'default.cfg'
  iotaapOs.getSystemParameter("timezone",timezone); // Get 'timezone' parameter from 'default.cfg'
  
  Serial.println("device_id parameter:");
  Serial.println(deviceId);
  Serial.println("timezone parameter:");
  Serial.println(*timezone); 
}

void loop()
{
  delay(500);
} 
```

System parameters are very useful for when we want to query and include which device / group is sending the message. Or even to which MQTT server the device is trying to connect. This troubleshooting knowledge is critical when diagnosing connection issues.

The result should appear similar to this.
```cpp
device_id parameter:
60366a8f5969ed0013207581
timezone parameter:
1
```

## User Parameters

Operating similarly to System parameters except reading from the `custom.cfg` file within the IoTaaP OS file system.
The custom.cfg can be edited to include any JSON formatted key pairs. This allows for easily configuration for any application specific information allowing a really flexible use-case.

It is important we make sure that any Char Arrays you are reading parameters into are large enough to hold them.

```cpp
#include <IoTaaP_OS.h>

IoTaaP_OS iotaapOs("1.0.1");

void setup()
{
  char ownerString[128]; // Char array used to store custom parameter
  iotaapOs.getUserParameter("owner", ownerString); // Get 'owner' parameter from 'custom.cfg'
  Serial.println("owner parameter:");
  Serial.println(ownerString);
}

void loop()
{
} 
```
This will result in returning the Owner string and printing the value on device setup.

Note although any pair may be added to the JSON structure, this must be completed into the file and cannot be written to via the OS object. The retrieval of parameters is one way. 


To write to the conguration files there is several options.
For Version 3.0.3 and below by using the [Configuration Wizard](https://docs.iotaap.io/docs-iotaap-os/config-wizard/).
For Version 3.1.0 and above by the use of the IoTaaP OS - Web Configurator.
Alternatively the SD card may be removed and placed into an SD Card reader and the .cfg files edited using a text editor. 

Custom parameters often include descriptions of the device location, Version etc and can be very flexible of types INT, Bool and Char (String).