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

:arrow_forward: Connect **Grove Rotary Angle Sensor** to analog pin **A0** of the Grove Base Shield.

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

### Write the Code to Read the Temperature Sensor.

Update <span class="icon file">temperature</span> to read the temperature sensor on program start up and log it to the console.

1.  Include the following headers in your C program

  	```c
    #include <iostream>
    #include <stdio.h>

    #include "temperature.hpp"
    #include "upm_utilities.h"
    ```

2.  Finally create a while loop that will continuously read the temperature value from sensor in celsius, convert it to fahrenheit and then display this on LCD by setting the cursor position and then writing the string. Also you can continuously change the LCD display color as done in the code.

3.  The final code should look like this:

``` c
#include <iostream>
#include <stdio.h>

#include "temperature.hpp"
#include "upm_utilities.h"

using namespace std;

#define PLATFORM_OFFSET 512
#define PIN 2 + PLATFORM_OFFSET

int
main()
{
  // Set the subplatform for the shield
  mraa_add_subplatform(MRAA_GROVEPI, "0");

  // Create the temperature sensor object using AIO pin 0
  upm::Temperature temp(PIN);
  std::cout << temp.name() << std::endl;

  // Read the temperature ten times, printing both the Celsius and
  // equivalent Fahrenheit temperature, waiting one second between readings
  for (int i = 0; i < 10; i++) {
      int celsius = temp.value();
      int fahrenheit = (int) (celsius * 9.0 / 5.0 + 32.0);
      printf("%d degrees Celsius, or %d degrees Fahrenheit\n", celsius, fahrenheit);
      upm_delay(1);
  }

  // Delete the temperature sensor object
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
