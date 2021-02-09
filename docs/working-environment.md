# Working Environment

Platformio is a really well known open source ecosystem for IoT development. Most advanced features are remote unit testing and firmware updates, which means that you will learn how to use pretty powerful cross-platform IDE.

## Installing VS Code

Platformio can be used in several code editors, but we will use Visual Studio Code. VS Code is great source code editor developed by Microsoft and it can be used on Windows, Linux or macOS. Great thing about VS Code is that it supports debugging, version control, intelligent code completion, snippets and much more.

In this step we will [**download**](https://code.visualstudio.com/) and install VS Code. Select your platform (Windows, Linux, macOS) and install your software after downloading it.

## Installing Platformio

Open Visual Studio Code that you have just installed and in the extensions field search for official **PlatformIO IDE** extension. Install PlatformIO extension.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/working-environment/platformio-ide-vscode-pkg-installer.png "PlatformIO IDE extension")

## Setting Up Platformio IDE

You can always access PlatformIO (PIO) home by clicking home button that can be found in the status bar of the VS Code.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/working-environment/platformio-home-300x278.png "PIO home")

## Adding IoTaaP platform to PIO

PlatformIO can work with various development boards, like Arduino, Nucleo, etc. In order to make your IoTaaP board function properly, you have to add it’s configuration to your PlatformIO installation.

Go to your **Platformio Home** tab in VS Code, select **Platforms** and click on the **Embedded tab**, then type **Espressif32** in the search bar.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/working-environment/iotaap-platformio-add-platform.png "Add platform")

Click on the **Espressif 32** in your search results and click the Install button.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/working-environment/iotaap-install-button-platformio.png "Install button platformio")

This will automatically install all needed files, settings and configurations for your **IoTaaP Magnolia** board.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/working-environment/iotaap-platformio-success.png "Success")

After installation, you should get the message that your platform is successfully installed.

You can check the official PlatformIO documentation about IoTaaP Magnolia board [**here**](https://docs.platformio.org/en/latest/boards/espressif32/iotaap_magnolia.html).

Now you can proceed with the next step and install IoTaaP library.

## Installing IoTaaP Libraries

In order to use all IoTaaP features out of the box, you have to install IoTaaP libraries.

Go to your **Platformio Home** tab and select **Libraries** from the side menu. Type “IoTaaP” in the search field, click on the specific IoTaaP Library, like **IoTaaP OS** by IoTaaP Team and then click **Add to Project**.

## New Project

Click on the platformio **Home** button and then click on **New Project** button in Quick Access menu.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/working-environment/platformio-home-new-project-1024x363.jpg "New Project")

In **Project Wizard** you will be able to enter your project name, select your development board and framework.

Type your project name (e.g. MyCoolProject) and search for IoTaaP Magnolia board*

Click on **Finish** and wait a few moments for PlatformIO to configure your new project

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/working-environment/new-project-created.jpg"New Project created")

Your project structure is very simple, you will mostly use **‘lib‘** and **‘src‘** directories. Expand **src** directory, and you will find _**main.cpp**_ file, this is the main file of your new project with the following content:

## Testing your environment

!!! info "IoTaaP Boards"
    Our development boards, like [IoTaaP Magnolia](https://www.iotaap.io/boards/#wifi) contain FT232RL - USB to serial converter onboard, and the following environment testing is applicable to this kind of development board. If you have different development board, containing ESP32 module, you should check official documentation from the manufacturer.

Connect your IoTaaP board with your PC using USB cable. Your PC should automatically install all drivers needed for your board to work. If you have trouble with automatic driver installation, you can [install your drivers manually](https://www.ftdichip.com/Drivers/VCP.htm)

PlatformIO automatically detects upload port by default. You can configure a custom port using ``` upload_port ``` option in **platformio.ini**

Use _Upload_ button in your Platformio status bar to start uploading your project to the board.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/working-environment/platformio-status-bar.jpg"Platformio status bar")

If everything works well you should see [SUCCESS] message. Congrats, your working environment is now fully functional 💪

**Doesn’t work?**

Feel free to write your problems in our [ **community** page.](https://community.iotaap.io/)

_**Manual port configuration:**_

Open **platformio.ini** file located in your project tree and add the following line (replace COM2 with your COM port if needed)

```bash
; COM2
upload_port = COM2
```
