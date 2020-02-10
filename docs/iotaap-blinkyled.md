# IoTaaP - Blinky LED

As one of the basic things to do while starting with new product development is to do “Blinky LED” test. This will also be your first working project. So let’s get started!

## Create new project

Create your new project in platformio in the same way we have done it in [**Working Environment**](https://www.iotaap.io/instructions/working-environment/) instructions. Just be sure to select your IoTaaP board and you are ready to go.

## Initial code setup

Initial code contains two **void** functions _setup()_ and _loop()_. First, one will run only once, and it’s usually used for initial configuration. The second one will run your code repeatedly (in a loop) which makes sense because our code will be running on an embedded system.

Let’s start by defining our LED. As we are writing our code in C++ language we will use best practices to keep our code clean and understandable as much as possible.

```cpp
#include "IoTaaP.h"

#define LED 26

IoTaaP iotaap;

void setup()
{
}

void loop()
{
}
```

With _#define_ we will tell our compiler that anywhere where we write **LED** it must replace it with **26** during compilation. This practice will help us later to easily change our LED pin without need for editing every pin number reoccurrence in the code.

## Defining pin as output

Now let’s define our pin as _OUTPUT_, every pin needs to be defined before usage.

```cpp
#include "IoTaaP.h"

#define LED 26

IoTaaP iotaap;

void setup()
{
  // Define output pin
  iotaap.misc.makePinOutput(LED);
}

void loop()
{
}
```

After defining our pin as OUTPUT, we can proceed with writing our Blinky LED algorithm. Let’s say that we want to turn our LED on for 0.5 seconds, and then off for 0.5 seconds, and repeat this. Functions that we will use are setPin(), clearPin() and delay(). Try to write code on your own and find the solution in the next step.

## Full Blinky LED code

Here is the full Blinky LED code:

```cpp
#include "IoTaaP.h"

#define LED 26

IoTaaP iotaap;

void setup()
{
  // Define output pin
  iotaap.misc.makePinOutput(LED);
}

void loop()
{
  // Turn the LED on by making voltage level HIGH
  iotaap.misc.setPin(LED);
  // Wait for 0.5 seconds
  delay(500);
  // Turn the LED off by making voltage level LOW
  iotaap.misc.clearPin(LED);
  // Wait for 0.5 seconds
  delay(500);
}
```
Now, upload your code to IoTaaP board as we have done it in [**Working Environment**](https://www.iotaap.io/instructions/working-environment/) instructions.

If you have any problems with your code, please feel free to create a new topic in our [ **community page**.](https://community.iotaap.io/)

## Customization

Now change your code to blink your LED in 1 second intervals (1 second on, 1 second off). Post your solutions in our  [ **community page**.](https://community.iotaap.io/)

Also, you can create your own rail crossing lights by adding one more LED to your code. Post your solution in our [**Coding section**](https://community.iotaap.io/c/iotaap/coding). If you need help, ask your fellow creators!