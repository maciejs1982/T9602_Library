# T9602_Library

***Arduino library interface for the [Telaire T9602 humidity and temperature sensor](https://www.amphenol-sensors.com/en/telaire/humidity/527-humidity-sensors/3224-t9602)***

![T9602 from Telaire](https://www.amphenol-sensors.com/images/stories/moisture-humidity/main-T9602-Mod-4.png)

***T9602 sensor.*** *Image from Amphenol/Telaire (see link above).*

## Key features

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
