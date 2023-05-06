# ESP32 pin schema

The controller IO pins are assigned according to the table below[^1][^2][^3].

|Function|Pin|Comment|
|------------------------------|:--:|--------|
||16|
||17|Used for ethernet port if enabled
||18|Used for ethernet port if enabled
||19|
||22|
||23|Used for ethernet port if enabled
|Mixing valve actuator decrease|25|
|Mixing valve actuator increase|26|
|Distributor line temperature sensors (DS18B20)|27|
|Outdoor temperature sensor (DS18B20)|14|
|Outdoor temperature sensor (NTC)|34|Downstream configuration with 5.66kOhm resistance at 5°C
|Outdoor temperature sensor power (NTC)|13|Supply +3.3V over a 1.0kOhm resistor
|System status LED|32|
|Controller status LED|33|
|Battery voltage|35|May require soldering on the ESP32 board to enable|
|Charging voltage|39|May require soldering on the ESP32 board to enable|

[^1]: [ESP32-Devkit-Lipo GPIOs map](https://www.olimex.com/Products/IoT/ESP32/ESP32-DevKit-LiPo/resources/ESP32-DevKit-Lipo-GPIOs.png)
[^2]: [ESP32-POE-ISO GPIO map](https://www.olimex.com/Products/IoT/ESP32/ESP32-POE-ISO/resources/ESP32-POE-ISO-GPIO.png)
[^3]: [ESP32 Pinout Reference: Which GPIO pins should you use?](https://randomnerdtutorials.com/esp32-pinout-reference-gpios/)
