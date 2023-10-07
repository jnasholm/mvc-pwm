# mvc-pwm
<!-- [![GitHub release (latest by date)](https://img.shields.io/github/v/release/jnasholm/mvc-pwm)](https://github.com/jnasholm/mvc-pwm/releases) -->
<!-- ![GitHub last commit](https://img.shields.io/github/last-commit/jnasholm/mvc-pwm) -->

# ESP32 SSR Mixing valve actuator controller

Project to create a smart mixing valve actuator controller for private homes with a hydronic heating system. Brief project description:

- The controller is an [ESP32](https://www.olimex.com/Products/IoT/ESP32/ESP32-DevKit-LiPo/open-source-hardware) programmed with [ESPHome](https://esphome.io/).
- The supply line temperature adjustment is based on ESPHome PID control[^1][^2].
- The [mixing valve actuator](https://esbe.eu/group/products/rotary-actuators/ara600-3-point) is an electric motor operated with 24 VAC.
- The switching modules are equipped with [solid state relays](https://www.velleman.eu/products/view/?id=461412).
- The controller can operate in automatic, semi-automatic or manual mode. Automatic mode with outdoor temperature compensated supply line temperature control. Semi-automatic mode with thermostatic supply line temperature set-point. Manual mode with static mixing valve position and manual increase/decrease.
- Line temperature sensors are digital [DS18B20](https://www.energibutiken.se/sv/dallas-1-wire-givare/10-dallas-1-wire-pro-rorgivare-02006.html) sensors.
- Outdoor temperature sensor is either an analog [NTC](https://en.wikipedia.org/wiki/Thermistor) or a digital [DS18B20](https://www.energibutiken.se/sv/dallas-1-wire-givare/24-dallas-1-wire-pro-utegivare-02002.html).
- Integration with [Home Assistant](https://www.home-assistant.io/) is recommended. Sensor data and configuration parameters can be imported to operate the controller.

[ESP32 SSR Floor Heating Controller](https://github.com/jnasholm/fhc-pwm)

## Description
The controller is designed to be a drop-in replacement for the legacy UVEAB EVR-CDR digital valve actuator controller. This controller has a classic outdoor temperature sensor EVR-U with a thermal resistor (NTC) to compensate the supply line temperature set-point. For closed loop feed-back to the controller another sensor EVR-F, also a thermal resistor (NTC), is attached to the supply line for the hydronic floor heating manifold. An indoor temperature sensor EVR-T was also available as an option but will not be covered here. The controller has four configurable operational parameter settings through manual potentiometers, heating gain curve, parallel curve displacement, actuator pulse interval, and night reduction. My controller had the gain set to 0.8, the displacement set to -2.5, pulse interval set to 20 s, and night reduction set to 0, when it was replaced with the smart controller.

In the set-up of my smart controller the supply line temperature sensor is replaced with a modern digital temperature sensor. The existing outdoor temperature sensor is reused without modification. Also the mixing valve actuator is reused. The mixing valve actuator is an ESBE ARA600 operated as a reversible 24 VAC electric motor with 90° rotational travel in 120 s.

The controller as two principal modes of operation, automatic and manual.
### Automatic mode
This is the active state (1) for the actuator. The thermostat function is in effect and compensation based on outdoor temperature is performed. The actuator is operated completely based on parameter settings. The actuator is run with the pulse interval configured through the front-end.

The actuator operation can be temporarily paused if necessary. This is the position hold state (4) for the actuator. No position adjustment of the actuator is performed. This function can be called from the front-end.

One practical scenario is if the temperature on the mixing valve inlet is not enough above the temperature set-point for the supply line. The controllability of the supply line temperature will then be low to none, and increasing the actuator position is pointless. When enough positive temperature difference is achieved, then the position hold can be released from the font-end, and the controller resumes automatic operation.

### Semi-automatic mode
This is the active state (2) for the actuator. The thermostat function is in effect but no compensation based on outdoor temperature is performed. The actuator is operated with a static set-point value through the front-end. The actuator is run with the pulse interval configured through the front-end.

### Manual mode
This is the idle state (0) for the actuator. No thermostat function is in effect and no compensation based on outdoor temperature is performed. The actuator is operated completely through manual switches, one for increasing the actuator position, one for decreasing the actuator position, and a stop button. When running this is the manual state (3) for the actuator.

The actuator is run with a fixed 1 s pulse interval, which is quite fast for hydronic floor heating systems. This can be used to quickly reposition the actuator if it has reached an unexpected or extreme position for some reason. During the initial configuration and adjustment of the control parameters, sometimes you end up with a severely oscillating, fully open, or fully closed actuator position by mistake. Switch to manual mode to regain some form of steady state, reconfigure and try again.

## Front-end configuration and control

**Actuator pulse interval:** Adjustable between 2 and 20 s. Decrease value to make the actuator faster in response, increase to make the actuator slower in response. Too small value can cause oscillation, too large can case over- or under-shoot of set-point. Default is 6 s.

**Controller mode:** Switch on for automatic mode, switch off for manual mode. Default is on.

**Outdoor temperature compensation:** Switch on for compensation, switch off for no compensation. Default is on in automatic mode.

**Position hold:** Switch on to enable actuator position hold, switch off to release actuator position hold. Default is off.

**Temperature compensation set-points:** Generally known as “heat curve”. A set of points between which the target supply line temperature is calculated based on meassured outdoor temperature. Calculation is based on linear interpolation. Available points are p1 to p6. Default point set values are:

| |Outdoor temperature|Target supply line temperature|
|---|:-------:|:-------:|
| |°C|°C|
|p1|-10|29.0|
|p2|-5|26.8|
|p3|0|25.0|
|p4|5|23.8|
|p5|10|23.0|
|p6|20|22.0|

**Note:** The controller hardware is theoretically capable of operating 110-230 VAC mixing valve actuators. This will however not be tested in the project.

[^1]: [PI Parameter Influence on Underfloor Heating Energy Consumption and Setpoint Tracking in nZEBs](https://www.mdpi.com/1996-1073/13/8/2068)
[^2]: [Reglering, om P-, I-, D-bidraget](https://www.bastec.se/anvandarmanual/reglering-p-i-d-bidraget/)
