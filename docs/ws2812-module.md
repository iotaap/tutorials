# WS2812 Module

WS2812 LED Module consists of 8 digital LEDs. This module gives you the possibility to interact with the World in a colorful way. 

!!! warning "Jumper configuration"
    WS2812 LED Module is connected to the P2 pin on IoTaaP Embedded connector. In order to use it you have to switch the jumper J1 to the 
    left position.

WS2812 is the most widespread digital LED and you can find a bunch of libraries for it, the most advanced library is: **Adafruit NeoPixel** by Adafruit.
Simply search for the library in the PlatformIO repository and install it. 

We will write a simple program that will loop Red, Green and Blue colors on all 8 LEDs. 

```cpp
#include <Adafruit_NeoPixel.h>
#include <IoTaaP.h>

IoTaaP iotaap;

#define PIN 2

Adafruit_NeoPixel strip = Adafruit_NeoPixel(8, PIN, NEO_GRB + NEO_KHZ800);

void colorWipe(uint32_t c, uint8_t wait)
{
  for (uint16_t i = 0; i < strip.numPixels(); i++)
  {
    strip.setPixelColor(i, c);
    strip.show();
    delay(wait);
  }
}

void setup()
{

  strip.begin();
  strip.setBrightness(50);
  strip.show();
}

void loop()
{
  colorWipe(strip.Color(255, 0, 0), 50); // Red
  colorWipe(strip.Color(0, 255, 0), 50); // Green
  colorWipe(strip.Color(0, 0, 255), 50); // Blue
}
```

We have to define IoTaaP object in order to initialize the board properly. Also, by defining PIN as 2 we will tell the board
to use P2 output on th IoTaaP Embedded connector. 

!!! info "Possible firmware upload failure"
    It is possible that from time to time your upload process will fail because this module uses P2 pin with voltage shifter on
    the module board which causes failure of the ESP32 module to switch to bootloader. We are aware of this issue and it will
    be fixed in the future versions of WS2812 LED Module. 

    Current workaround for this issue is to take out the jumper J1 during uploading of the new firmware.

