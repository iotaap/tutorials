# Servo Motor Module

Servo Motor Module is based on PCA9685 PWM driver and it can drive up to 6 servo motors powered by standard voltages.

!!! warning "High power module"
    This module can be powered from IoTaaP board or from an external source. If you are using more servos with high power connect the external 5V power 
    supply as noticed in the documentation.

PCA9685 PWM driver is using I2C communication and default module address is 0x40, you can change it by soldering onboard solder jumpers. 

In this example we will write a simple program that will move 1 servo motor from max left to the max right position (please configure min and max pulse
length to match your servo motor).

```cpp
#include <IoTaaP.h>
#include <Wire.h>
#include <Adafruit_PWMServoDriver.h>

Adafruit_PWMServoDriver pwm = Adafruit_PWMServoDriver(0x40);
IoTaaP iotaap;

uint8_t servonum = 0;
#define SERVOMIN  150 // This is the 'minimum' pulse length count (out of 4096)
#define SERVOMAX  600 // This is the 'maximum' pulse length count (out of 4096)
#define SERVO_FREQ 50 // Analog servos run at ~50 Hz updates

void setup()
{
  pwm.begin();
  pwm.setOscillatorFrequency(27000000);  // Configuring internal oscillator of th Servo driver
  pwm.setPWMFreq(SERVO_FREQ);  // Analog servos run at ~50 Hz updates
  delay(10);
}
void loop()
{
    for (uint16_t pulselen = SERVOMIN; pulselen < SERVOMAX; pulselen++) {
    pwm.setPWM(servonum, 0, pulselen);
  }

  delay(150);
  for (uint16_t pulselen = SERVOMAX; pulselen > SERVOMIN; pulselen--) {
    pwm.setPWM(servonum, 0, pulselen);
  }

  delay(150);

  servonum++;
  if (servonum > 1) servonum = 0; // Testing 1 servo, increase number if you are using more servos
}
```

You can connect up to 6 servo motors to this module. If there is more than 1 servo connected just increase 
the number of servos in the last if clause:

```cpp
if (servonum > 3) servonum = 0; // For 3 servo motors
```


