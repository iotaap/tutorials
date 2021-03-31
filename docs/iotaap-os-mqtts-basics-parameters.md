# IoTaaP OS - Parameters

## What is a parameter?

A parameter is a value of a specific type used within a system to convey information.

IoTaaP OS uses internal FAT File system to store many of these parameters. Specifically under the /ffat directory. Within the default.cfg file.


## System Parameters

These are critical system informations. Often unique to the device.
A collection of these are read from the the internal file structure by the system. This is where the default.cfg file holds the system parameters.

From this .cfg file the system can read the JSON structure allowing us to call and use these parameters.

To call a system parameter we use the function:

`iotaapOs.getSystemParameter("device_id", deviceId);`

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

  iotaapOs.start(); // Start IoTaaP OS  

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

!!! info "Comming soon"
    User parameters are not available in IoTaaP OS v4.0.0 release, since some braking changes, where configuration system is transffered
    to local FAT filesystem. We are working on the implementation of user parameters to be directly set and retreived from the main code.


To write to the conguration files there is several options.
For Version 3.0.3 and below by using the [Configuration Wizard](https://docs.iotaap.io/docs-iotaap-os/config-wizard/).
For Version 3.1.0 and above by the use of the IoTaaP OS - Web Configurator.