# IoTaaP - TSL2561

The  **TSL2561** is a light-to-digital converter that transforms light intensity to a digital signal output capable of direct I2C interface. This is an example that will show you how to use 3rd party libraries with IoTaaP.

## Library installation

In order to use [**TSL2561**](https://cdn-shop.adafruit.com/datasheets/TSL2561.pdf) we have to install it’s library to our PlatformIO workspace. Go to the platformio Home tab, click Libraries and in the search field search for “**SparkFun TSL2561**” this is an open source library that works with IoTaaP. Install the library and create a new project.

## Coding

We will write a code that will initialize our sensor and print lux values to the serial port. Default code from this example uses 0 gain and 402ms integration time (you can read more about this values in the datasheet).

```cpp
#include <IoTaaP.h> 
#include <SparkFunTSL2561.h> 

IoTaaP iotaap;
SFE_TSL2561 light;

boolean gain;    // Gain setting, 0 = X1, 1 = X16;
unsigned int ms; // Integration ("shutter") time in milliseconds

double lux;   // Resulting lux value
boolean good; // True if sensor is saturated

void setup()
{
  // Initialize the Serial port:
  iotaap.serial.init(115200);
  iotaap.serial.printLn("TSL2561 example");

  // Initialize the TSL2561
  light.begin();

  // If gain = false (0), device is set to low gain (1X)
  // If gain = high (1), device is set to high gain (16X)
  gain = 0;

  // If time = 0, integration will be 13.7ms
  // If time = 1, integration will be 101ms
  // If time = 2, integration will be 402ms
  unsigned char time = 2;

  // setTiming() will set the third parameter (ms) to the
  // requested integration time in ms (this will be useful later):
  iotaap.serial.printLn("Set timing...");
  light.setTiming(gain, time, ms);

  // To start taking measurements, power up the sensor:
  iotaap.serial.printLn("Powerup...");
  light.setPowerUp();
}

void loop()
{
  delay(ms);
  
  unsigned int data0, data1;

  if (light.getData(data0, data1))
  {

    // Perform lux calculation:
    good = light.getLux(gain, ms, data0, data1, lux);

    // Print out the results:
    iotaap.serial.printLn("lux: ");
    iotaap.serial.printLn(String(lux));
    if (good)
      iotaap.serial.printLn("GOOD");
    else
      iotaap.serial.printLn("BAD");
  }
  else
  {
    iotaap.serial.printLn("Error");
  }
}
```


## Explanation

In this example you can see how easily you can integrate 3rd party libraries with IoTaaP.

Remember that all IoTaaP examples are available in the “examples” directory of your installed IoTaaP library.