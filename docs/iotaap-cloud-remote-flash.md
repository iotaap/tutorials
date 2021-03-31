# IoTaaP OS Remote flash

Starting from IoTaaP OS version 4, and IoTaaP Cloud version 2, remote flashing option is available. This feature gives you the possibility to remotely flash you **ESP32** device that runs IoTaaP OS. Please note that
you have to flash IoTaaP OS to your device, and [configure it](iotaap-os-esp32-web-config.md) using parameters provided in the IoTaaP Cloud console.

### Firmware - firmware.bin

Everything starts with **firmware.bin** file, this file contains your compiled code, it can be some official release of our modules firmware or your custom firmware. It is important that your new firmware
uses IoTaaP OS (latest version is recommended), otherwise you will lose access to your device. 

If you are using some official firmware by IoTaaP (or authorized partners) than you can skip the following steps,
but if you are using your own solution then you first have to compile your code using PlatformIO (which is recommended development framework).

First you have to **Build** your firmware by clicking on the checkmark icon in PlatformIO interface

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/remote-flash/compile.PNG "Compile code")

After successfull compilation, your **firmware.bin** will be available under `.pio/build/release` directory

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/remote-flash/firmware_bin.PNG "firmware.bin")

After successfull remote flashing, your device will restart, and you will see new **Firmware version** in the device details.

## Device remote flash

If automatic OTA updates are enabled, your system will check for new updates every 6 hours, this is sometimes good option when you are deploying group updates, or fast reaction is not required. But mostly, 
you will need to push new updates instantly, especially during development, if your device is already installed in some system or similar. 

### Flashing using IoTaaP Cloud Console

Select your device in IoTaaP Console. You will see your device details and MQTTS connection menu, where you have to connect to the **same** MQTTS server as your device. If your device is turned on, you will be able
to see the device status, firmware version and other parameters. 

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/remote-flash/device_status.PNG "ESP32 device status")

Next, you have to drag and drop your **firmware.bin** to the upload field, and enter your firmware version. **Version should match your version defined in the main code** `IoTaaP_OS iotaapOs("1.0.7");` After uploading, click on the **Submit** button, and your firmware will be ready for flashing.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/remote-flash/firmware_uploaded_device.PNG "Firmware uploaded")

You will notice that **Firmware version** field will change color to red, which indicates that firmware on the device is different then the firmware you have uploaded. Click on the **Flash** button, to start the update procedure. 

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/remote-flash/flash_button.PNG "Flash button")

## Group remote flash

If you have multiple devices with the same firmware, you can add them to a **Group** and flash thousands of devices at the same time, without cables and programmers. 

### Flashing using IoTaaP Cloud Console

Select your group of devices in the console by clicking on the green action button (Devices List). This will open list of devices that are currently available in your group. In order to start flashing procedure, you have
to connect to the **same** MQTTS server as your device. 

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/remote-flash/mqtt_server_connect.PNG "MQTT Server Connect")

Next, drag and drop your **firmware.bin** to the upload field in the console, and enter your firmware version. **Version should match your version defined in the main code** `IoTaaP_OS iotaapOs("1.0.7");` After uploading, click on the **Submit** button, and your firmware will be ready for flashing.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/remote-flash/firmware_uploaded.PNG "Firmware uploaded")

Now you are ready to start your group flashing by clicking on the **Flash** button. Your device will start the update procedure instantly. 

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/remote-flash/flash_button.PNG "Flash button")

You can monitor update status, and overall device status in device details interface.

## Firmware version

Please ensure that you enter the same version to the **Version** field in the console, and as fwVersion parameter in your main code. 

Also, after successfull update and restart, at each IoTaaP OS boot, you will be able to see the current firmware version, as well as IoTaaP OS version

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/remote-flash/web-configurator-credentials.PNG "IoTaaP OS - Serial output")

!!! info "Notice"
    Please note that you will not be able to flash single device if it's a part of the group, even if you remove it from the group in the console. Device
    can be configured as a stand-alone device or as a part of the group. This configuration can be updated using [IoTaaP OS Web Configurator](iotaap-os-esp32-web-config.md).