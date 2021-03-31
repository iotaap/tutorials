# JSON
Within the IoTaaP Ecosystem we are required to send data beween devices. To keep a standard format we use JSON for all Machine to Machine (M2M) exchanges.

## Why JSON and how it works.
JavaScript Object Notation allows us to pass information in an easily machine-readable format.
JSON is a international standard used for its simplicity. JSON is in essence a list of paired values.

Normally the first value is a category, with the second being the value.

`{"category" : "value" }`

JSON can also have multiple pairs of values within an object.

```cpp
{
    "category_1" : "value_1",
    "category_2" : "value_2",
    "category_3" : "value_3"
}
```
## Examples using Arduino JSON
To parse data into JSON format we suggest including the [Arduino JSON library](https://arduinojson.org/) to parse data into JSON format.

This can be achieved by adding `ArduinoJson` to the Lib_deps within the platformio.ini file within the project.

```
lib_deps = 
    iotaap/IoTaaP OS @ ^3.0.2 
    arduinoJson
```
First we need to include the ArduinoJson library within our main.cpp.
`#include <ArduinoJson.h>` is added to the top.

Followed by declaring an JSONDocument object with a 200 item capacity.
```cpp
StaticJsonDocument<200> jdoc;
```

Next we need a Char Array of suitable size to hold our JSON Object.
```cpp
char json_char[256];
```

Within the loop we need to add our parameters into the json document.
```cpp
jdoc["date"]= "2020-01-01";
jdoc["time"]= "00:01:01";
```
There is several formats which can be used. More details can be found within [arduinoJson Docs](https://arduinojson.org/v6/doc/serialization/).

Finally we can use the "serialize" command from the arduinoJson library to convert this JSON doc into a char array in json format.

```cpp
serializeJson(jdoc, json_char);
```

The JSON Code as a full example.

```cpp
#include <IoTaaP_OS.h>
#include <ArduinoJson.h>

IoTaaP_OS iotaapOs("1.0.1");
StaticJsonDocument<200> jdoc; //Json document with capacity 200
char json_char[255]; // Json char array with max size of 256 chars

void callback(char *topic, byte *message, unsigned int length)
{
  Serial.println("---------------------------");
  Serial.println("Received data on topic:");
  Serial.println(topic); // Print topic

  Serial.println("Data:");

  for (int i = 0; i < length; i++) // Print message
  {
    Serial.print((char)message[i]);
  }
  Serial.println();
  Serial.println("---------------------------");
}

void setup()
{
  iotaapOs.startWifi(); // Connect to WiFi
  iotaapOs.startMqtt(callback); // Connect to MQTT broker
  delay(1000);
}

void loop()
{
  jdoc["date"]= "2020-01-01";
  jdoc["time"]="00:01:01";
  serializeJson(jdoc, json_char);
  Serial.println(json_char);
  delay(1000);
}
```
## End result
Serial output of Json Object should appear like so.
```cpp
{"date":"2020-01-01","time":"00:01:01"}
{"date":"2020-01-01","time":"00:01:01"}
```
Now we are familiar with JSON and can convert values into JSON formatted objects. we can now progress to the different MQTT publishes.