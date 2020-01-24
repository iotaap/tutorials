# PWM (Pulse Width Modulation)

PWM  (Pulse Width Modulation) is a way to control analog devices with a digital output. In this guide we will generate 1kHz frequency PWM signal.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/pwm/iotaap-oscilloscope-pwm.gif "PWM Signal")

## Coding

We will generate PWM signal with frequency of 1kHz. Our values will go from 0% to 100% and back to 0%.

```cpp
#include "IoTaaP.h"

IoTaaP iotaap;

void setup()
{
  // Configure PWM output
  iotaap.pwm.setup(1, 1000, 8, 26);
}

void loop()
{
  for (int i = 0; i <= 255; i++)
  {
    // Set PWM duty cycle
    iotaap.pwm.set(1, i);
    delay(10);
  }
  for (int i = 255; i >= 0; i--)
  {
    iotaap.pwm.set(1, i);
    delay(10);
  }
}
```

## Explanation

To test your code you can connect a LED to your pin (26 in our example) and you will se how LED changes its brightness. You can see how PWM looks like on our oscilloscope.

```cpp
iotaap.pwm.setup(1, 1000, 8, 26);
```

Before using PWM we must configure it. setup() function expects few mandatory parameters

1. Channel – PWM channel that defines on which channel PWM will be generated (values from 1 to 15) – value 0 is used by onboard buzzer
2. Frequency – Frequency of our PWM signal
3. Resolution – Signal resolution, the higher the value the more pricise output  (1 – 16)
4. Pin – Pin where PWM channel will reproduce output (any digital output pin)

```cpp
iotaap.pwm.set(1, i);
```

This line will set the duty of PWM pulse to value of ‘i’ (0-255). First argument is channel (must be the same as defined in setup) and duty value in expected range (0-255 for 8 bit, 0-65535 for 16 bit).
