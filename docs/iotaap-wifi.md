# IoTaaP - WiFi

WiFi is really crucial feature of IoTaaP Magnolia board. In this guide we will learn how to scan for WiFi networks, list them and connect to the selected AP.

## Coding

With IoTaaP it’s really easy to connect to the WiFi network. Check out the code below and in the next step you will find some explanations. Be sure to update your code with appropriate password for you SSID. Also, if you have some questions you can always [ask our community!](https://community.iotaap.io/)
```cpp
#include "IoTaaP.h"

IoTaaP iotaap;

void setup()
{
  // Init oled and show text
  iotaap.oled.init();
  iotaap.oled.showText("Scanning...");

  // Scann WiFi AP's and return number of discovered
  int numOfAP = iotaap.wifi.scan();

  // Create list of available networks
  String networks = "Available networks:\n\n";

  for (int i = 0; i < numOfAP; i++)
  {
    networks += String(i + 1) + ". " + iotaap.wifi.getScanned(i) + "\n";
  }

  // Show discovered networks
  iotaap.oled.showText(networks);
  delay(3000);

  // Connect to the first network discovered
  iotaap.oled.showText("Connecting to: " + iotaap.wifi.getScanned(0));

  // Convert String type SSID to char type SSID
  char SSID[64];
  iotaap.wifi.getScanned(0).toCharArray(SSID, 64);

  // Declare wifiInfo structure
  wifiConnectionInfo wifiInfo;

  // Connect to the SSID with Password (must be valid password)
  wifiInfo = iotaap.wifi.connect(SSID, "password");
  delay(1000);

  // Check if IoTaaP is connected to the WiFi network
  if (wifiInfo.status == 1)
  {
    // Display IP address if successfully connected
    iotaap.oled.showText("Connected!\n\nIP: " + wifiInfo.ipAddress.toString());
  }
  else
  {
    // Display message if there was a problem
    iotaap.oled.showText("There was a problem");
  }
}

void loop()
{
  delay(500);
}
```

## Explanation

Let’s go through the code together.
```cpp
int numOfAP = iotaap.wifi.scan();
```
This line of code will scan for WiFi networks and return number of discovered networks.
```cpp
iotaap.wifi.getScanned(i)
```
We can use _numOfAP_ in the line of code above to go through discovered networks, this dunction will return SSID as a String.
```cpp
wifiInfo = iotaap.wifi.connect(SSID, "password");
```
After we select our SSID and password, IoTaaP will try to connect to the AP with provided credentials. Timeout is 5s, which means that the function will return status=-1 after 5 seconds if it was unable to connect to the AP.
```cpp
struct wifiConnectionInfo
{
    int status;
    IPAddress ipAddress;
};
```
WiFi status is available under **wifiConnectionInfo** structure. Status will be -1 if there was an error and IP will be 0.0.0.0, in this case IP address should be ignored. If connecting was successfull, status will be 1, and IP address will be the current IP address obtained.

**IoTaaP as a WiFi access point**

IoTaaP can be used as an access point. In order to open access point you should call the following function:
```cpp
iotaap.wifi.openAP("AP_NAME","MyPASSWORD");
```
This will simply configure IoTaaP to work as an AP with the provided name. If you want to create an open AP, only parameter you have to provide is AP name:
```cpp
iotaap.wifi.openAP("AP_NAME");
```