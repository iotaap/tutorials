# IoTaaP - Pins

IoTaaP has various I/O pins that can be configured for different features, such as PWM, DAC, SPI, ADC, etc. Some pins have some special behaviours, so be sure to check the table below. Get more info about pin configuration at [IoTaaP Embedded Standard page](https://www.iotaap.io/iotaap-embedded/).

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/iotaap-pins/iotaap-embedded-standard.jpg"IoTaaP Embedded")


| **PIN**     | **Input** | **Output** | **Notes**                                  |
| ----------- | --------- | ---------- | ------------------------------------------ |
| VSYS        | N/A       | N/A        | External power supply (5V MAX)             |
| 5VOUT       | N/A       | N/A        | 5Vout (1A MAX)                             |
| GND         | N/A       | N/A        | GND                                        |
| 3.3V        | N/A       | N/A        | 3.3Vout (300mA MAX)                        |
| RESET       | N/A       | N/A        | LOW level will reset module                |
| P2          | YES       | YES        | P2 or LED2                                 |
| P4          | YES       | YES        | P4                                         |
| P12         | YES*      | YES        | Boot will fail if HIGH                     |
| P13         | YES       | YES        | P13                                        |
| P14         | YES       | YES        | Outputs PWM at boot                        |
| P15         | YES       | YES        | Outputs PWM at boot. P15 or LED1           |
| P16         | YES       | YES        | P16                                        |
| P17         | YES       | YES        | P17                                        |
| SCL (I2C)   | YES       | YES        | P22 (4k7 pull-up to 3.3V)                  |
| SDA (I2C)   | YES       | YES        | P21 (4k7 pull-up to 3.3V)                  |
| MISO (SPI)  | YES       | YES        | P19 with 100R resistor                     |
| MOSI (SPI)  | YES       | YES        | P23 with 100R resistor                     |
| CLK (SPI)   | YES       | YES        | P18 with 100R resistor                     |
| CS  (SPI)   | YES       | YES        | P5 with 100R resistor. Outputs PWM at boot |
| TX (Serial) | TX pin    | YES*       | Debug output at boot                       |
| RX (Serial) | YES*      | RX pin     | HIGH at boot                               |
| P26         | YES       | YES        |                                            |
| P27         | YES       | YES        |                                            |
| P32         | YES       | YES        | P32 or Buzzer                              |
| P33         | YES       | YES        | P33                                        |
| P34         | YES       | NO         | P34                                        |
| P35         | YES       | NO         | P35                                        |
| A3          | YES       | NO         | Analog input                               |