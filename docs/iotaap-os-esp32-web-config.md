# IoTaaP OS Web Configurator

[IoTaaP OS](https://docs.iotaap.io/docs-iotaap-os/) Web Configurator is an amazing feature that gives you the possibility to configure your **ESP32** based device
without cables, serial port connections and similar.

## Entering IoTaaP OS Web Configurator

During operation or boot, you have to press the predefined “Config button” on the board (routed to GPIO 25) - *please note that IoTaaP OS uses inverted logic, so 10k pull-up resistor to 3.3V is required, and configuration button is active low* 

By pressing "Config button" device will enter into Access Point (AP) mode and start the configurator. 

In the future, every device provided by us will come with a printed SSID and Password, but in order to find your device access data, you can get the SSID and Password of your device during boot (after pressing config button) through the serial port. 

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/remote-flash/web-configurator-credentials.PNG "Web configurator credentials")

During Web configurator operation, onboard LED will blink in a predefined pattern.

![alt text](https://community.iotaap.io/uploads/default/original/1X/a9f746e3c0fdf96fcd19348929f8d6a9a4e15635.png "IoTaaP OS Access Point")

## Opening Web Configurator

Connect to your IoTaaP SSID and head to http://192.168.1.8, this will open the IoTaaP Web configurator:

![alt text](https://community.iotaap.io/uploads/default/original/1X/d73cc88337efc226885ed22b0178aa183bb20865.png "IoTaaP ESP32 Web Configurator")

Enter your parameters from the IoTaaP Console, click **Confirm**, and your device will automatically restart and start operating with the new configuration.

![alt text](https://community.iotaap.io/uploads/default/original/1X/9e20567c5fb6db23ff856bac641a2c643b1dd147.png "ESP32 Web Configurator success")

## CA certificate

You can get the certificate [here](https://docs.iotaap.io/docs-iotaap-os/certificates/).