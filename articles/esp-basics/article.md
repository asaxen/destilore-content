# ESP Tutorial
This tutorial is about setting up Esp8266 with platformio


## Hardware

We will use the esp8266 module:

![alt text](../../media/esp/esp_basics.jpg "Esp8266")

## Prerequisites

You will need:
- Esp8266
- Platformio
- Happy mood

## Code

```c
#include <Arduino.h>

void setup()
{
    Serial.begin(9600);
}

void loop()
{
    Serial.println("Hello world!");
    delay(1000);
}
```

## Build and flash

```ssh
platformio run -t upload
```

## A table
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |