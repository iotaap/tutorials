# IoTaaP - DAC

DAC (Digital To Analog Converter)  is a system that converts a digital signal into an analog signal.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/iotaap-dac/20190920_210218-300x169.gif"DAC gif")

In this guide we will generate voltages from 0 to 3.3V with onboard DAC (pin 26).

## Coding

We will generate voltages from 0V to 3.3V and back to 0V with onboard DAC. Onboard DAC has resolution of 255bits and it’s attached to pin 26.
```cpp
#include "IoTaaP.h"

IoTaaP iotaap;

void setup()
{
}

void loop()
{
  for (int i = 0; i <= 255; i++)
  {
    // Set DAC value
    iotaap.dac.set(i);
    delay(10);
  }
  for (int i = 255; i >= 0; i--)
  {
    iotaap.dac.set(i);
    delay(10);
  }
}
```
## Explanation

To test your code you can connect a LED to the DAC pin (26) and you will see how LED changes its brightness. You can see how DAC output looks like on our oscilloscope. Unlike PWM, DAC generates “real voltages” while PWM voltages are simulated by using pulse width (and peak to peak voltages are still Vmax)
```cpp
iotaap.dac.set(i);
```
Also, unlike PWM, our DAC does not require any setup, you just have to pass value from 0 to 255 and it will generate voltage accordingly (0 is 0V, 255 is Vcc = 3.3V)