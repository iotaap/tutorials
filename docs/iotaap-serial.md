# IoTaaP - Serial

**UART** stands for Universal Asynchronous Receiver/Transmitter. It’s not a communication protocol like SPI, but a physical circuit in a MCU (microcontroller unit), or a stand-alone chip. A UART’s main purpose is to transmit and receive serial data. In this guide we will create a program that will send some message to the PC and then return back received data.

## Coding 

We will create a simple code that will initialize our serial port to use 115200 baud. After initialization code sends message “IoTaaP Serial Communication” to the port and finally it will send back every character received back to the sender.

```cpp
#include "IoTaaP.h"

IoTaaP iotaap;

void setup()
{
  // Initialize IoTaaP serial port with 115200 baud rate  
  iotaap.serial.init(115200);
  // Print line to the serial port
  iotaap.serial.printLn("IoTaaP Serial Communication");
  // Delay for 1 second
  delay(1000);
}

void loop()
{
  // If character is received, send it back
  if(iotaap.serial.isAvailable()){
    iotaap.serial.writeChar(iotaap.serial.readChar());
  }
}
```
## Testing

In order to test our code we need a terminal software. We recommend [Termite](https://www.compuphase.com/software_termite.htm). If you already have some serial terminal software on your PC, feel free to use it.

Configure your serial terminal software to use the port where your IoTaaP module is connected (you can check this in your Device manager if you are using Windows). As IoTaaP is using FTDI’s **FT232** for USB to serial conversion, Windows and other systems like Linux will install drivers automatically.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/iotaap-serial/iotaap-termite-terminal-configure.jpg"IoTaaP Termite terminal configure")

Be sure to configure baud to 115200, the same as in your code, of course you can use different speed, but you must have the same value both in your code and your terminal software.

Upload your code to the IoTaaP, connect your terminal software to the port and press Reset button on your IoTaaP board, you will se the message in your terminal software. Send some data to the IoTaaP, and it will return it back to your terminal.

You can get data like below when resetting your IoTaaP board, but this is normal behavior:

```
rst:0x1 (POWERON_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
configsip: 0, SPIWP:0xee
clk_drv:0x00,q_drv:0x00,d_drv:0x00,cs0_drv:0x00,hd_drv:0x00,wp_drv:0x00
mode:DIO, clock div:2
load:0x3fff0018,len:4
load:0x3fff001c,len:1100
load:0x40078000,len:9232
load:0x40080400,len:6412
entry 0x400806a8
```

Feel free to play with other serial functions, and post your experiments to our [ **community page**.](https://community.iotaap.io/)