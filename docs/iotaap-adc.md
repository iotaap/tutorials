# IoTaaP - ADC

**Analog to Digital Converter (ADC)** is a system that converts an analog signal, such as a voltage, into a digital signal, just opposite of the [DAC](https://www.iotaap.io/instructions/iotaap-dac/). Our onboard controller (ESP32) has 12bit ADC onboard, on all pins but it also has a pretty non-linear characteristics as you can see on the chart below:

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/iotaap-adc/ADC-non-linear-ESP32.png"ADC-non-linear-ESP32") 

[Source](https://github.com/espressif/arduino-esp32/issues/92)

In this guide we will read RAW values from the pin, and convert them to the voltage.

## Coding

We will write a simple code that will read RAW data from the pin 26 (potentiometer attached) and convert RAW data to the voltage value. Both values will be printed on the OLED.

```cpp
#include "IoTaaP.h"

IoTaaP iotaap;

void setup()
{
  iotaap.oled.init();
}

void loop()
{
  // Get RAW value from pin 26
  uint16_t value = iotaap.adc.getValue(26);
  // Convert RAW value to voltage
  double voltage = iotaap.adc.getVoltage(value);
  // Show data on the OLED
  iotaap.oled.showText("RAW: " + String(value) + "\nVoltage: " + String(voltage));
  delay(100);
}
```
## Explanation

As mentioned in the introduction, our main controller ESP32 has pretty non-linear ADC, but it’s just enough for the most aplications.

With IoTaaP you do not need to initialize or configure ADC, you just have to use one function for reading 12bit data from the specified pin:

```cpp
uint16_t value = iotaap.adc.getValue(26);
```
This line of code will read analog value from the pin 26 and save it to the ‘value’ variable.
```cpp
double voltage = iotaap.adc.getVoltage(value);
```
This line of code will convert RAW analog value to the voltage. This function contains an empirical formula that compensates the error of ADC, so you will get a little bit more accurate voltage values.

_getVoltage()_ function looks like this:
```cpp
double IoTaaP_ADC::getVoltage(int reading)
{
    if (reading < 1 || reading > 4095)
        return 0;
    return (-0.000000000000016 * pow(reading, 4) + 0.000000000118171 * pow(reading, 3) - 0.000000301211691 * pow(reading, 2) + 0.001109019271794 * reading + 0.034143524634089);
}
```