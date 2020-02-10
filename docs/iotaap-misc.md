# IoTaaP - MISC

All common functions are contained in IoTaaP_Misc object. With those functions you can use most basic features like onboard LED’s, Buttons or control pin states.

## Basic features

The following code will turn the LED1 on if BUT1 is pressed and LED2 if BUT2 is pressed, also if battery level is below 5%, IoTaaP will be put to sleep. At the beggining onboard LED1 will blink 5 times with 100mS delay and for demonstration purposes we will make pin 26 as input, and pin 27 as output. Notice that we are using _**getBut1()**_ in the first condition and _**getPin()**_ in the second condition! This functions are practically the same but _**getPin()**_ will get state of any digital input pin.

```cpp
#include "IoTaaP.h"

IoTaaP iotaap;

void setup()
{
  // Blink onboard LED
  iotaap.misc.blinkOnboardLed(ONBOARD_LED1, 100, 5);
  // Make P26 as input
  iotaap.misc.makePinInput(26);
  // Make P27 as output
  iotaap.misc.makePinOutput(27);
}

void loop()
{
  // Check if BUT1 is pressed and turn on LED1 if true
  if (iotaap.misc.getBUT1())
  {
    iotaap.misc.setPin(ONBOARD_LED1);
  }
  // If BUT1 is not pressed turn off LED1
  else
  {
    iotaap.misc.clearPin(ONBOARD_LED1);
  }
  // Check if BUT2 is pressed and turn on LED2 if true
  if (!iotaap.misc.getPin(ONBOARD_BUT2))
  {
    iotaap.misc.setPin(ONBOARD_LED2);
  }
  // If BUT2 is not pressed turn off LED2
  else
  {
    iotaap.misc.clearPin(ONBOARD_LED2);
  }
  // If battery percentage is < 5, put IoTaaP to sleep for 1 minute
  if (iotaap.misc.getBatteryPercentage() < 5)
  {
    iotaap.misc.sleep(60);
  }
}
```
All other example codes are available in _examples_ directory inside IoTaaP library.