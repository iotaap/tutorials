# IoTaaP - Hall

A Hall effect sensor is a device that is used to measure the variations in a magnetic field. In this really simple guide we will read Hall sensor values and show them on the OLED.

Function _iotaap.hall.read()_ is the only function you will need to get the reading from onboard Hall sensor. Check out the code below:
```cpp
#include "IoTaaP.h"

IoTaaP iotaap;

void setup()
{
  iotaap.oled.init();
}

void loop()
{
  iotaap.oled.showText(String(iotaap.hall.read()));
  delay(200);
}
```
