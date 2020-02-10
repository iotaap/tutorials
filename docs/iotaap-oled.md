# IoTaaP - OLED

In order to show some information to the user we can use OLED instead of just using LED’s. OLED that you can find on IoTaaP Magnolia board has a **SSD1306** controller and resolution of 128x64px which is just enough for most of the applications.

## Coding

In this basic OLED example we will show some text and then we will countdown from 10 to 0 with 500ms delay, after that we will show IoTaaP logo on the display.

```cpp
#include "IoTaaP.h"

IoTaaP iotaap;

void setup()
{
  // Initialize OLED
  iotaap.oled.init();
  // Show some text
  iotaap.oled.showText("Countdown", 40, 30);
  delay(1500);
}

void loop()
{

  for (int i = 10; i >= 0; i--)
  {
    iotaap.oled.showText(String(i), 60, 32);
    delay(500);
  }
  // Display IoTaaP logo
  iotaap.oled.displayBitmap(0, 0, iotaap_splash_128x64, 1);
  while (1);
}
```
If we are planning to use OLED feature, we have to initialize it with _**init()**_ function. Function _**showText()**_ will display any text at given coordinates and _**displayBitmap()**_ will display any 128x64px bitmap. You can use [**LCD Image Converter**](https://sourceforge.net/projects/lcd-image-converter/files/) for converting image into an array.

## LCD Image Converter

As mentioned in previous step, you can use LCD Image Converter in order to convert your image to C++ array and display it on the OLED.

Short manual can be found [**here**](https://www.riuson.com/lcd-image-converter/short-manual).