# T9602_Library

***Arduino library interface for the [Telaire T9602 humidity and temperature sensor](https://www.amphenol-sensors.com/en/telaire/humidity/527-humidity-sensors/3224-t9602)***

![T9602 from Telaire](https://www.amphenol-sensors.com/images/stories/moisture-humidity/main-T9602-Mod-4.png)

***T9602 sensor.*** *Image from Amphenol/Telaire (see link above).*

## Library

[Library reference documentation](library_reference.md) generated by doxygen and moxygen.

* Class description
* Function descriptions

## Key sensor features

  * IP67 rated
  * -20 to 70 °C operating range
  * Accuracy
    * ±2% RH, 0-95% RH
    * ±0.5°C
  * 14 bit resolution

[Data sheet](https://www.amphenol-sensors.com/en/component/edocman/304-telaire-t9602-humidity-temperature-sensor-datasheet/download?Itemid=8487%20%27) for the full T9602 series.

[Application guide for the ChipCap® 2 Humidity and Temperature Sensor](https://www.amphenol-sensors.com/en/component/edocman/397-telaire-chipcap2-humidity-and-temperature-sensor-application-guide/download?Itemid=8487%20%27) (core unit within the T9602). Importantly, this includes a guide to the I2C bus. This library operates only using the default I2C address (0x28), so you will need to consult this and this library (and make a pull request, please!) if you want to allow for different addresses to be set/used.

## Sensor options

Our two most commonly used sensors are:
  * [T9602-3-D](https://www.digikey.com/product-detail/en/amphenol-advanced-sensors/T9602-3-D/235-1563-ND/5027914)
    * I2C
    * 3.3V
    * 1.8 m cable
  * [T9602-3-D-1](https://www.digikey.com/product-detail/en/amphenol-advanced-sensors/T9602-3-D-1/235-1377-ND/5027895)
    * I2C
    * 3.3V
    * 1.0 m cable

5V versions, as well as those with other communications protocols, [are available as well](https://www.digikey.com/products/en/sensors-transducers/humidity-moisture-sensors/529?k=t9602).

## Wiring

**_WARNING_: LAYOUT DIFFERS FROM OUR STANDARD CONVENTIONS**

| **Color** | **Connection** |
|-----------|----------------|
| Red       | V+             |
| Green     | GND            |
| White     | SDA            |
| Black     | SCL            |

We typically snip off the connector at and wire the sensor directly into the screw terminals of our data logger ([Resnik](https://github.com/NorthernWidget-Skunkworks/Project-Resnik) or [Margay](https://github.com/NorthernWidget-Skunkworks/Project-Margay)) or into our [Heptapod](https://github.com/NorthernWidget-Skunkworks/Project-Heptapod/) I2C expander. You may instead choose to find a mating plug; the wires feel a bit delicate, but in our experience hold up better than expected.

![T9602 Digi-Key image with connector](https://media.digikey.com/photos/Amphenol%20Photos/T9602%20SERIES.jpg)

***T9602 and connector.*** *Image from Digi-Key (see links above).*

## Writing a program to connect to the T9602

You should be able to use any standard Arduino device to connect to the T9602 and read its data.

*This code is untested. If you do test it, please confirm that it works and/or create a PR and/or an issue for it to be fixed.*

### Very simple Arduino code

This code is intended for any generic Arduino system.

```c++
// Include the T9602 library
#include "T9602.h"

// Declare variables -- just as strings
String header;
String data;

// Instantiate class
T9602 mySensor;

void setup(){
    // Begin Serial connection to computer at 38400 baud
    Serial.begin(38400);
    // Obtain the header just once
    header = mySensor.getHeader();
    // Print the header to the serial monitor
    Serial.println(header);
}

void loop(){
    // Take one reading every (10 + time to take reading) seconds
    // and print it to the screen
    mySensor.updateMeasurements();
    data = mySensor.getString();
    Serial.println(data);
    delay(10000); // Wait 10 seconds before the next reading, inefficiently
}
```

### Northern Widget Margay code

The [Margay data logger](github.com/NorthernWidget-Skunkworks/Project-Margay) is the lightweight and low-power open-source data-logging option from Northern Widget. It saves data to a local SD card and includes on-board status measurements and a low-drift real-time clock. We have written [a library to interface with the Margay](github.com/NorthernWidget-Skunkworks/Margay_Library), which can in turn be used to link the Margay with sensors.

```c++
// Include the T9602 library
#include "Margay.h"
#include "T9602.h"

// Declare variables -- just as strings
String header;
String data;

// Instantiate classes
T9602 mySensor;
Margay Logger(Model_2v0, Build_B); // Margay v2.2; UPDATE CODE TO INDICATE THIS

// Empty header to start; will include sensor labels and information
String Header = "";

// I2C address for T9602
uint8_t I2CVals[] = {0x28};

//Number of seconds between readings
uint32_t updateRate = 60;

void setup(){
    Header = Header + mySensor.getHeader();
    Logger.begin(I2CVals, sizeof(I2CVals), Header);
    initialize();
}

void loop(){
    initialize();
    Logger.Run(update, updateRate);
}

String update() {
    initialize();
    return mySensor.getString();
}

void initialize(){
    mySensor.begin();
}
```

## Acknowledgments

Support for this project provided by:

<img src="https://pbs.twimg.com/profile_images/1139626463932637186/qCak0yvY_400x400.png" alt="UMN ESCI" width="240px">
<br/>
<br/>
<img src="https://www.nsf.gov/news/mmg/media/images/nsf_logo_f_ba321daf-8607-41d7-94bc-1db6039d7893.jpg" alt="NSF" width="240px">
