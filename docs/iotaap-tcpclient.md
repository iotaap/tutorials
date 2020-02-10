# IoTaaP - TCP Client

IoTaaP embeds default TCP server and TCP client objects that can be used directly in the code.

**TCP** (Transmission Control Protocol) is a standard that defines how to establish and maintain a network conversation via which application programs can exchange data. TCP works with the **Internet Protocol** (IP), which defines how computers send packets of data to each other.

## Coding

In this guide we will create a program that will connect to local WiFi network and then connect to the TCP server running on PC on port 5656. You can check the code below and test it, if there are some questions you can discuss them with the [community](https://community.iotaap.io/). In the next step we will go through the code.

```cpp
#include "IoTaaP.h"

IoTaaP iotaap;

void setup()
{
  // Init OLED
  iotaap.oled.init();
  // Connect to the WiFi AP
  iotaap.wifi.connect("SSID", "PASSWORD");
  // Connect to the TCP server
  iotaap.tcp.connect("YOUR-PC-IP-ADDRESS", 5656);
}

void loop()
{
  char data[255];
  // Read string from TCP server
  iotaap.tcp.readString('\0').toCharArray(data, 255);
  // Show received string on the OLED
  iotaap.oled.showText(data);
  // Send received string back to the server
  iotaap.tcp.send(data);

  // If string equals to ON, blink onboard LED1
  if (String(data) == "ON")
  {
    iotaap.misc.blinkOnboardLed(ONBOARD_LED1);
  }
}
```

## Explanation

We will start a TCP server on our PC with [**Hercules**](https://www.hw-group.com/software/hercules-setup-utility).

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/iotaap-tcpclient/hercules-tcp-server.jpg"IoTaaP TCP connect")

```cpp
iotaap.tcp.connect("192.168.8.103", 5656);
```
Be sure to put your PC’s IP address to the connect function, since our TCP server will run on the PC.

```cpp
iotaap.tcp.readString('\0')
```
This line will read data from the server until ‘\0’ is received.

```cpp
iotaap.tcp.send(data);
```
This line will send received data to the server.

**Testing the code**

In order to test the code we have to use TCP Server software or program, we recommend [**Hercules**](https://www.hw-group.com/software/hercules-setup-utility).This is the most complete terminal software for testing connectivity. But there are a lot of various solutions on the internet for this purpose.

Install Hercules on your PC, and run it, PC and IoTaaP MUST be on the same network! Click on TCP Server tab and enter port 5656, click Listen. You will be able to send messages to the connected clients, and receive messages from the clients. Congrats, you have built your TCP client application with IoTaaP!

Post your modifications and examples to our [**community page**](https://community.iotaap.io/).