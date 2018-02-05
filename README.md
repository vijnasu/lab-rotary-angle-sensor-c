# Using the Rotary Angle Sensor in C

## Objective

During the first step in this lab, you will compare a small analog program using the Arduino APIs to an program using the UPM Grove Temperature libary.

You will write code in C to connect a rotary angle sensor using upm library and printing the values to the serial monitor.

## Setup the Rotary Angle Sensor
## Hardware requirements

### Grove\*

Sensor | Pin
--- | ---
Rotary Angle Sensor | A0

![](./images/action.png) Connect **Grove Rotary Angle Sensor** to analog pin **A0** of the Grove Base Shield.

## Analog I/O using the Arduino API
Create a new project
```c
void setup() {
  mraa_add_subplatform(MRAA_GROVEPI, "0");
  DebugSerial.begin(115200);
}

void loop() {
  int sensorValue = analogRead(514);
  DebugSerial.println(sensorValue);
  delay(100);
}
```

This program will get the raw value from the sensor and display it as a number between 0 and 1023.  

Let's compare it with a program written using UPM.

## Rotary Angle Sensor Program using UPM

:arrow_forward: Create a new program.

### Write the Code to Read the Rotary Angle Sensor.

``` c

#include <iostream>
#include <stdint.h>
#include <stdio.h>
#include <string>

#include "groverotary.hpp"
#include "upm_utilities.h"

using namespace std;

int
main()
{
    //! [Interesting]
    // Instantiate a rotary sensor on analog pin A0
    upm::GroveRotary knob(0);

    // Print sensor name to confirm it initialized properly
    cout << knob.name() << endl;

    while (true) {
        float abs_value = knob.abs_value(); // Absolute raw value
        float abs_deg = knob.abs_deg();     // Absolute degrees
        float abs_rad = knob.abs_rad();     // Absolute radians
        float rel_value = knob.rel_value(); // Relative raw value
        float rel_deg = knob.rel_deg();     // Relative degrees
        float rel_rad = knob.rel_rad();     // Relative radians

        fprintf(stdout,
                "Absolute: %4d raw %5.2f deg = %3.2f rad Relative: %4d raw %5.2f "
                "deg %3.2f rad\n",
                (int16_t) abs_value,
                abs_deg,
                abs_rad,
                (int16_t) rel_value,
                rel_deg,
                rel_rad);

        upm_delay_us(2500000); // Sleep for 2.5s
    }
    //! [Interesting]
    return 0;
}
```

## Build and Upload your program
When you compile and run your program, you should see the sensor's value printing to your serial monitor or console.

There are a number of additional examples available for reference as [how-to-code-samples](https://github.com/intel-iot-devkit/how-to-code-samples) on Github.

## Additional resources

Information, community forums, articles, tutorials and more can be found at the [Intel Developer Zone](https://software.intel.com/iot).

For reference code for any sensor/actuator from the Grove* IoT Commercial Developer Kit, visit [https://software.intel.com/en-us/iot/hardware/sensors](https://software.intel.com/en-us/iot/hardware/sensors)
