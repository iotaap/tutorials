# Weather & SD Module (Weather Example)

Weather & SD Module incorporates Bosch's BME280 environmental sensor as well as micro SD card slot.

In this example we will cover usage of the BME280 environmental sensor (Weather part of the module). BME280 sensor supports both I2C and SPI communication protocols,
but this shield only incorporates I2C communication. Default address is 0x76, and it can be changed by shorting onboard jumper. 

In this example we will combine 3rd party libraries by Adafruit (Adafruit Unified Sensor, Adafruit BME280 Library) and IoTaaP Core library. Install the libraries using
PlatformIO registry. IoTaaP Core will be used to initialize the board and to write readings on the serial port. 

We will write a simple program that will read temperature, pressure, humidity and calcualte current altitude (based on the current pressure).

```cpp
#include <IoTaaP.h>
#include <Wire.h>
#include <SPI.h>
#include <Adafruit_BME280.h>

#define SEALEVELPRESSURE_HPA (1013.25)

IoTaaP iotaap;

Adafruit_BME280 bme; // I2C

void showMeasurements()
{
  iotaap.serial.printLn("Temperature: " + String(bme.readTemperature(), 2) + " *C");
  iotaap.serial.printLn("Pressure: " + String(bme.readPressure() / 100.0F, 2) + " hPa");
  iotaap.serial.printLn("Altitude: " + String(bme.readAltitude(SEALEVELPRESSURE_HPA), 2) + " m");
  iotaap.serial.printLn("Humidity: " + String(bme.readHumidity(), 2) + " %");

  iotaap.serial.printLn("");
}

void setup()
{
  iotaap.serial.init(9600);

  if (!bme.begin(BME280_ADDRESS_ALTERNATE))
  { // 0x76 is the default address on Weather & SD Module
    iotaap.serial.printLn("Could not find a valid BME280 sensor!");
    while (1)
      ;
  }

  iotaap.serial.printLn("");
}

void loop()
{
  showMeasurements();
  delay(1000);
}
```

If everything is right, your readings should look like this:

```
Temperature: 25.49 *C
Pressure: 1011.42 hPa
Altitude: 15.23 m
Humidity: 53.45 %
```

!!! info "Terminal software"
    In order to read your values you have to install any serial terminal on your PC. We suggest using **Termite**.