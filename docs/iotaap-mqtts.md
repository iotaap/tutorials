# IoTaaP - MQTTS

This example is pretty similar to the IoTaaP – MQTT. But pure MQTT communication is not safe, that’s why we prefer to use MQTTS (MQTT + SSL). In this example we will use Let’s Encrypt’s Cross Signed certificate, **IoTaaP Cloud**.

!!! warning "IoTaaP Cloud supports only MQTTS"
    Please note that from IoTaaP 2.0 release in January 2021, in order to enhance security of our system, IoTaaP Cloud supports only **MQTTS** connection and strictly defined **root topic** rules (every topic
    starts with your MQTTS username, e.g. `/<your-username>/<your-topic>`).

# Setup

We must define our features before _#include <IoTaaP.h>_

```cpp
#define ENABLE_JSON
```
In _**setup()**_ function we wil configure WiFi and connect IoTaaP to WiFi AP (with internet access)

```cpp
iotaap.wifi.connect("<your-ssid>","<your-password>");
```
Now it’s time to initialize MQTT object and connect IoTaaP to our MQTT Instance.

```cpp
iotaap.mqtt.connect("iotaap_client", "mqtt1.iotaap.io", 8883, callback, true, "USERNAME", "PASSWORD", CA_cert);
```
This line of code will connect ESP32 to the MQTT broker running on IoTaaP Cloud. Notice the new parameter here: **CA_cert**. In the IoTaaP – MQTT example **secure** parameter was set to **false**, now it’s set to **true** and we have to pass CA Certificate to the function.

In order to user CA Certificate you will have to define it just below **IoTaaP iotaap;** line in your code:

```cpp
const char *CA_cert = "-----BEGIN CERTIFICATE-----\n"
                      "MIIGEzCCA/ugAwIBAgIQfVtRJrR2uhHbdBYLvFMNpzANBgkqhkiG9w0BAQwFADCB\n"
                      "iDELMAkGA1UEBhMCVVMxEzARBgNVBAgTCk5ldyBKZXJzZXkxFDASBgNVBAcTC0pl\n"
                      "cnNleSBDaXR5MR4wHAYDVQQKExVUaGUgVVNFUlRSVVNUIE5ldHdvcmsxLjAsBgNV\n"
                      "BAMTJVVTRVJUcnVzdCBSU0EgQ2VydGlmaWNhdGlvbiBBdXRob3JpdHkwHhcNMTgx\n"
                      "MTAyMDAwMDAwWhcNMzAxMjMxMjM1OTU5WjCBjzELMAkGA1UEBhMCR0IxGzAZBgNV\n"
                      "BAgTEkdyZWF0ZXIgTWFuY2hlc3RlcjEQMA4GA1UEBxMHU2FsZm9yZDEYMBYGA1UE\n"
                      "ChMPU2VjdGlnbyBMaW1pdGVkMTcwNQYDVQQDEy5TZWN0aWdvIFJTQSBEb21haW4g\n"
                      "VmFsaWRhdGlvbiBTZWN1cmUgU2VydmVyIENBMIIBIjANBgkqhkiG9w0BAQEFAAOC\n"
                      "AQ8AMIIBCgKCAQEA1nMz1tc8INAA0hdFuNY+B6I/x0HuMjDJsGz99J/LEpgPLT+N\n"
                      "TQEMgg8Xf2Iu6bhIefsWg06t1zIlk7cHv7lQP6lMw0Aq6Tn/2YHKHxYyQdqAJrkj\n"
                      "eocgHuP/IJo8lURvh3UGkEC0MpMWCRAIIz7S3YcPb11RFGoKacVPAXJpz9OTTG0E\n"
                      "oKMbgn6xmrntxZ7FN3ifmgg0+1YuWMQJDgZkW7w33PGfKGioVrCSo1yfu4iYCBsk\n"
                      "Haswha6vsC6eep3BwEIc4gLw6uBK0u+QDrTBQBbwb4VCSmT3pDCg/r8uoydajotY\n"
                      "uK3DGReEY+1vVv2Dy2A0xHS+5p3b4eTlygxfFQIDAQABo4IBbjCCAWowHwYDVR0j\n"
                      "BBgwFoAUU3m/WqorSs9UgOHYm8Cd8rIDZsswHQYDVR0OBBYEFI2MXsRUrYrhd+mb\n"
                      "+ZsF4bgBjWHhMA4GA1UdDwEB/wQEAwIBhjASBgNVHRMBAf8ECDAGAQH/AgEAMB0G\n"
                      "A1UdJQQWMBQGCCsGAQUFBwMBBggrBgEFBQcDAjAbBgNVHSAEFDASMAYGBFUdIAAw\n"
                      "CAYGZ4EMAQIBMFAGA1UdHwRJMEcwRaBDoEGGP2h0dHA6Ly9jcmwudXNlcnRydXN0\n"
                      "LmNvbS9VU0VSVHJ1c3RSU0FDZXJ0aWZpY2F0aW9uQXV0aG9yaXR5LmNybDB2Bggr\n"
                      "BgEFBQcBAQRqMGgwPwYIKwYBBQUHMAKGM2h0dHA6Ly9jcnQudXNlcnRydXN0LmNv\n"
                      "bS9VU0VSVHJ1c3RSU0FBZGRUcnVzdENBLmNydDAlBggrBgEFBQcwAYYZaHR0cDov\n"
                      "L29jc3AudXNlcnRydXN0LmNvbTANBgkqhkiG9w0BAQwFAAOCAgEAMr9hvQ5Iw0/H\n"
                      "ukdN+Jx4GQHcEx2Ab/zDcLRSmjEzmldS+zGea6TvVKqJjUAXaPgREHzSyrHxVYbH\n"
                      "7rM2kYb2OVG/Rr8PoLq0935JxCo2F57kaDl6r5ROVm+yezu/Coa9zcV3HAO4OLGi\n"
                      "H19+24rcRki2aArPsrW04jTkZ6k4Zgle0rj8nSg6F0AnwnJOKf0hPHzPE/uWLMUx\n"
                      "RP0T7dWbqWlod3zu4f+k+TY4CFM5ooQ0nBnzvg6s1SQ36yOoeNDT5++SR2RiOSLv\n"
                      "xvcRviKFxmZEJCaOEDKNyJOuB56DPi/Z+fVGjmO+wea03KbNIaiGCpXZLoUmGv38\n"
                      "sbZXQm2V0TP2ORQGgkE49Y9Y3IBbpNV9lXj9p5v//cWoaasm56ekBYdbqbe4oyAL\n"
                      "l6lFhd2zi+WJN44pDfwGF/Y4QA5C5BIG+3vzxhFoYt/jmPQT2BVPi7Fp2RBgvGQq\n"
                      "6jG35LWjOhSbJuMLe/0CjraZwTiXWTb2qHSihrZe68Zk6s+go/lunrotEbaGmAhY\n"
                      "LcmsJWTyXnW0OMGuf1pGg+pRyrbxmRE1a6Vqe8YAsOf4vmSyrcjC8azjUeqkk+B5\n"
                      "yOGBQMkKW+ESPMFgKuOXwIlCypTPRpgSabuY0MLTDXJLR27lk8QyKGOHQ+SwMj4K\n"
                      "00u/I5sUKUErmgQfky3xxzlIPK1aEn8=\n"
                      "-----END CERTIFICATE-----\n";
```

Our system is fully protected by Sectigo.

![alt text](https://files.iotaap.io/assets/iotaap-os/assets/sectigo_seal.png )

**Root Certificate**

If you need root certificate mentioned above, you can get it [here](https://files.iotaap.io/assets/iotaap-os/assets/ca.crt).

!!! warning "Invalid certificate"
    System will not be able to connect to the MQTT server or do the OTA update if certificate is missing or invalid.

**Notice that we are using ‘\n’ and this is the only way to make your certificate work properly, so if you are using different certificate make sure to keep the same formatting.** 

**callback()** is the function that will be called every time when something arrives to our subscribed topic(s). We will define this function later.

```cpp
iotaap.mqtt.subscribe("/<your-username>/ledstatus/led1");
```
This line of code will subscribe IoTaaP to `/<your-username>/ledstatus/led1` topic. We will define our callback in the next step.

*Keep in mind that every CA Certificate or Root certificate has it’s own expiration date. If the certificate is not valid or if it’s expired your MQTTS connection will not work! This is completely automated and covered with our [IoTaaP OS](https://docs.iotaap.io/docs-iotaap-os/).*

## Callback function

Callback function signature is already defined so it cannot be changed and it looks like this:

```cpp
void callback(char* topic, byte* message, unsigned int length) 
```
Now we will define the function body:

```cpp
void callback(char* topic, byte* message, unsigned int length) {
  String messageTemp;
  
  for (int i = 0; i < length; i++) {
    messageTemp += (char)message[i];
  }

  if (String(topic) == "/<your-username>/ledstatus/led1") {
    if(messageTemp == "on"){
      iotaap.misc.setPin(ONBOARD_LED1);
    }
    else if(messageTemp == "off"){
      iotaap.misc.clearPin(ONBOARD_LED1);
    }
  }
}
```
This function will convert our message into **String** type and it will check if our trigger topic was `/<your-username>/ledstatus/led1`, if this is true, our function will check message content. Content must be “on” in order to turn the LED1 on, or “off” to turn it off.

## Loop

!!! info "IoTaaP Magnolia V2 development board"
    Please note that our production version of IoTaaP Magnolia Development Board does not contain accelerometer anymore, and accelerometer option is removed
    from the IoTaaP Core library. But this example is a good starting point for your own implementation.

In our main loop we will publish our accelerometer data in jSON format, and we will call mandatory function keepAlive() that will keep our MQTT connection active.

IoTaaP library comes with ArduinoJson (C++ JSON library for Arduino and IoT) for easier handling of the jSON format. We will use only serializeJson() function in this example, but feel free to check other features of ArduinoJson library.

```cpp
void loop()
{
  iotaap.mqtt.keepAlive();

  doc["x"] = 510;
  doc["y"] = 10;
  doc["z"] = 24;

  char accJson[255];

  serializeJson(doc, accJson);

  iotaap.mqtt.publish("/<your-username>/iotaap/accelerometer", accJson, false);

  delay(200);
}
```
This code will keep our MQTT connection active, but it will also read accelerometer data. Axis values will be saved to “doc” with appropriate axis names. Function serializeJson(doc,accJson); will convert **doc** to jSON string, and finally `iotaap.mqtt.publish(“/<your-username>/iotaap/accelerometer”, accJson);` will publish values to `/<your-username>/iotaap/accelerometer` topic.

![alt text](https://files.iotaap.io/assets/iotaap-tutorials/iotaap-mqtt/iotaap-accelerometer-topic-1024x368.jpg"IoTaaP/accelerometer")

## Testing

We will use MQTT.fx for testing. Simply connect to your **IoTaaP Cloud** (or any other MQTT broker) instance and publish on or off to `/<your-username>/ledstatus/led1` topic. Be sure to subscribe to the `/<your-username>/iotaap/accelerometer` topic and you will se accelerometer coordinates in jSON format

```json
{
  "x" : 510,
  "y" : 10,
  "z" : 24
}
```
Now, create a code that will control 2 LED’s and publish button status, share your solutions with the [community](https://community.iotaap.io/).
