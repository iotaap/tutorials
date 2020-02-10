# IoTaaP - Buzzer

It is really easy to generate tones with onboard buzzer. You can use onboard buzzer in order to signal some events, create an alarm or some interactive game. In this guide you will learn how to configure and use IoTaaP onboard buzzer.

## Coding

We will write a code that will initialize buzzer and generate one tone with 2400Hz frequency that will last for 300mS. After that our code should generate frequency sweep from 0 to 20kHz.
```cpp
#include "IoTaaP.h"

IoTaaP iotaap;

void setup()
{
  // Initialize buzzer
  iotaap.buzzer.initBuzzer();
  // Generate tone with 2400Hz frequency that will last for 300mS
  iotaap.buzzer.generateTone(2400, false, 300);
}

void loop()
{
  for (int i = 0; i <= 20000; i = i + 10)
  {
    iotaap.buzzer.initBuzzer();
    iotaap.buzzer.generateTone(i, false, 10);
  }
  // Detach buzzer
  iotaap.buzzer.stopTone();
}
```
## Explanation

Onboard buzzer must be initialized before use with the following function:
```cpp
iotaap.buzzer.initBuzzer();
```
To generate tone we can use some variations of _generateTone()_ function.
```cpp
iotaap.buzzer.generateTone(2400, false, 300);
```
This line will generate one tone that will last for 300 mS. But if we want to generate tone that will last until _stopTone()_ function is called we can do the following:
```cpp
iotaap.buzzer.generateTone(2400, true);
```
When ‘forever’ parameter is set to ‘true’ tone will be generated until
```cpp
iotaap.buzzer.stopTone();
```
function is called. It’s a good practice to call stopTone() function even if ‘forever’ parameter is not true in order to release buzzer’s [PWM channel](https://www.iotaap.io/instructions/iotaap-pwm/).

Try to generate some melody using this feature and post your solutions to our [**community page**](https://community.iotaap.io/).