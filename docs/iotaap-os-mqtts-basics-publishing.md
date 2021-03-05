# Basic Examples

You will find an Initial example MQTT Code under the IoTaaP Library.

VSCode Folder Structure.

`PIO-> libdeps/IoTaaP-Magnolia/IoTaaP OS/ -> examples/initial_code `

This tutorial series breaks down this example and walks through the different MQTTS features of IoTaaP OS and how to use them.

```cpp

//insert code.
```

# Publishing

Publishing in MQTT is the action of creating a message and then sending that message through an MQTT broker to all subscribers of a topic.

There is multiple ways to publish supported within IoTaaP OS.
{More Details in the docs https://docs.iotaap.io/docs-iotaap-os/functions/#int-basiccloudpublish}

The simplest publish command is the basic cloud publish.

```cpp
iotaapOs.basicCloudPublish("{\"my_simple_text\":\"Hello IoT World!\"}", "simple_topic");
```

int basicCloudPublish(JSON_Object,topic);

## JSON

JavaScript Object Notation allows us to pass information in an easily machine-readable format.

In essence we are moving our data into the format.

`{"name" : "value" }`






