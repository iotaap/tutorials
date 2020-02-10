# IoTaaP -TCP Server

IoTaaP embeds default TCP server and TCP client objects that can be used directly in the code.

**TCP** (Transmission Control Protocol) is a standard that defines how to establish and maintain a network conversation via which application programs can exchange data. TCP works with the **Internet Protocol** (IP), which defines how computers send packets of data to each other.

# Coding

In this guide we will create a program that will connect to local WiFi network and start TCP server in port 344. If client connects to the port, server will receive data and send it back to the client. You can check the code below and test it, if there are some questions you can discuss them with the [community](https://community.iotaap.io/). In the next step we will go through the code.

```cpp
#include "IoTaaP.h"

IoTaaP iotaap;

void setup()
{
  // Init OLED
  iotaap.oled.init();
  // Connect to the WiFi AP
  iotaap.wifi.connect("SSID", "PASSWORD");
  // Show local IP address
  iotaap.oled.showText(WiFi.localIP().toString());
  // Start TCP server on port 344
  iotaap.wifiServer.begin(344);
}

void loop()
{
  // Check if client is requesting connection
  iotaap.wifiClient = iotaap.wifiServer.available();

  if (iotaap.wifiClient)
  {
    // Serve client while connected
    while (iotaap.wifiClient.connected())
    {

      while (iotaap.wifiClient.available() > 0)
      {
        char c = iotaap.wifiClient.read();
        iotaap.wifiClient.write(c);
      }

      delay(10);
    }
    // Close connection
    iotaap.wifiClient.stop();
  }
}
```

# Explanation

```cpp
WiFi.localIP()
```
This is the stock function that will fetch current IP address of IoTaaP module. We are showing this IP on the OLED to be able to connect to the server.

```cpp
iotaap.wifiServer.begin(344);
```
This line will start TCP server on port 344, we must be connected to the WiFi before this!

```cpp
iotaap.wifiClient = iotaap.wifiServer.available();
```
System will wait for the client to connect and then it will serve the client (as you can se in the while() operation)

**Testing the code**

In order to test the code we have to use TCP Client software or program, we recommend [**Hercules**](https://www.hw-group.com/software/hercules-setup-utility).This is the most complete terminal software for testing connectivity. But there are a lot of various solutions on the internet for this purpose.

Install Hercules on your PC, and run it, PC and IoTaaP MUST be on the same network! Click on TCP Client tab and enter IoTaaP’s IP address and port 344, click Connect. You will be able to send messages to the IoTaaP TCP server, and IoTaaP TCP server will return your messages back to you. Congrats, you have built your TCP server with IoTaaP!

Post your modifications and examples to our [**community page**](https://community.iotaap.io/).