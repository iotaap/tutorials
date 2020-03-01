# CAN Module

CAN Module is based on SN65HVD230 CAN transceiver. It can be used to interact with any standard CAN line.

In this example we will cover usage of this module with 2 IoTaaP Magnolia boards that will communicate together. First connect modules to boards and
connect them together using two wires. In this example we will use IoTaaP Core library in order to properly initialize IoTaaP board and ESP32CAN by iotsharing.com library
for CAN communication.

First module "the sender" will send 8 bytes of data (text "helloiot") in lowercase, second module "response module", will receive 8 bytes of data
and convert all lowercase letters to the uppercase leters. 

## Sender module

Let's start by writing a program that will send helloiot data.

```cpp
#include <IoTaaP.h>
#include <ESP32CAN.h>
#include <CAN_config.h>

// Defining CAN configuration variable
CAN_device_t CAN_cfg;

IoTaaP iotaap;

void setup()
{
  iotaap.serial.init(9600);

  CAN_cfg.speed = CAN_SPEED_1000KBPS; // CAN baud rate
  CAN_cfg.tx_pin_id = GPIO_NUM_26;    // CAN TX pin
  CAN_cfg.rx_pin_id = GPIO_NUM_27;    // CAN RX pin

  // CAN receiving queue
  CAN_cfg.rx_queue = xQueueCreate(10, sizeof(CAN_frame_t));
  // CAN Initialisation
  ESP32Can.CANInit();
}

void loop()
{
  CAN_frame_t rx_frame;
  // Get next CAN frame from queue
  if (xQueueReceive(CAN_cfg.rx_queue, &rx_frame, 3 * portTICK_PERIOD_MS) == pdTRUE)
  {

    // Process data
    if (rx_frame.FIR.B.FF == CAN_frame_std)
      iotaap.serial.printLn("New standard frame");
    else
      iotaap.serial.printLn("New extended frame");

    if (rx_frame.FIR.B.RTR == CAN_RTR)
      printf(" RTR from 0x%08x, DLC %d\r\n", rx_frame.MsgID, rx_frame.FIR.B.DLC); // Using printf in order to format received data
    else
    {
      printf(" from 0x%08x, DLC %d\n", rx_frame.MsgID, rx_frame.FIR.B.DLC); // Using printf in order to format received data
      for (int i = 0; i < 8; i++)
      {
        printf("%c\t", (char)rx_frame.data.u8[i]); // Using printf in order to format received data
      }
      iotaap.serial.printLn("");
    }
  }
  else
  {
    rx_frame.FIR.B.FF = CAN_frame_std;
    rx_frame.MsgID = 1;
    rx_frame.FIR.B.DLC = 8;
    rx_frame.data.u8[0] = 'h';
    rx_frame.data.u8[1] = 'e';
    rx_frame.data.u8[2] = 'l';
    rx_frame.data.u8[3] = 'l';
    rx_frame.data.u8[4] = 'o';
    rx_frame.data.u8[5] = 'i';
    rx_frame.data.u8[6] = 'o';
    rx_frame.data.u8[7] = 't';

    // Send response
    ESP32Can.CANWriteFrame(&rx_frame);
  }
}
```

First we need to define standard CAN frame and message ID in order to create a propper "package" with 8 bytes length, after that
we have to fill 8 frames with 8 byte data and finally write this frame to the CAN bus line.

## Response module

Now we will write a program that will receive 8 frames of data and convert the letters to the uppercase.

```cpp
#include <IoTaaP.h>
#include <ESP32CAN.h>
#include <CAN_config.h>

// Defining CAN configuration variable
CAN_device_t CAN_cfg;

IoTaaP iotaap;

void setup()
{
  iotaap.serial.init(9600);

  CAN_cfg.speed = CAN_SPEED_1000KBPS; // CAN baud rate
  CAN_cfg.tx_pin_id = GPIO_NUM_26;    // CAN TX pin
  CAN_cfg.rx_pin_id = GPIO_NUM_27;    // CAN RX pin

  // CAN receiving queue
  CAN_cfg.rx_queue = xQueueCreate(10, sizeof(CAN_frame_t));
  // CAN Initialisation
  ESP32Can.CANInit();
}

void loop()
{
  CAN_frame_t rx_frame;
  // Get next CAN frame from queue
  if (xQueueReceive(CAN_cfg.rx_queue, &rx_frame, 3 * portTICK_PERIOD_MS) == pdTRUE)
  {

    // Process data
    if (rx_frame.FIR.B.FF == CAN_frame_std)
      iotaap.serial.printLn("New standard frame");
    else
      iotaap.serial.printLn("New extended frame");

    if (rx_frame.FIR.B.RTR == CAN_RTR)
      printf(" RTR from 0x%08x, DLC %d\r\n", rx_frame.MsgID, rx_frame.FIR.B.DLC); // Using printf in order to format received data
    else
    {
      printf(" from 0x%08x, DLC %d\n", rx_frame.MsgID, rx_frame.FIR.B.DLC); // Using printf in order to format received data
      // Converting received character to uppercase and returning back to sender
      for (int i = 0; i < 8; i++)
      {
        if (rx_frame.data.u8[i] >= 'a' && rx_frame.data.u8[i] <= 'z')
        {
          rx_frame.data.u8[i] = rx_frame.data.u8[i] - 32;
        }
      }
    }
    // Send response
    ESP32Can.CANWriteFrame(&rx_frame);
  }
}
```

We have to define receive queue in order to receive data. *You can easily filter "important" messages by MsgID parameter*, in this example
our response module will process every message received. If you monitor this module with serial terminal you should see message ID and DLC (length) in
the following format:

```
New standard frame
 from 0x00000001, DLC 8
New standard frame
 from 0x00000001, DLC 8
```

If everything is right, your readings should look like this:

```

New standard frame
H	E	L	L	O	I	O	T	 from 0x00000001, DLC 8

New standard frame
H	E	L	L	O	I	O	T	 from 0x00000001, DLC 8

New standard frame
H	E	L	L	O	I	O	T	 from 0x00000001, DLC 8

New standard frame
H	E	L	L	O	I	O	T	 from 0x00000001, DLC 8

```

!!! info "Terminal software"
    In order to read your values you have to install any serial terminal on your PC. We suggest using **Termite**.