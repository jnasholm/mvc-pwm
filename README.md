# mvc-pwm
<!-- [![GitHub release (latest by date)](https://img.shields.io/github/v/release/jnasholm/mvc-pwm)](https://github.com/jnasholm/mvc-pwm/releases) -->
<!-- ![GitHub last commit](https://img.shields.io/github/last-commit/jnasholm/mvc-pwm) -->

# ESP32 SSR Mixing valve actuator controller

Project to create a smart mixing valve actuator controller for private homes with a hydronic underfloor system. Brief project description:

- The controller is an [ESP32](https://www.olimex.com/Products/IoT/ESP32/ESP32-DevKit-LiPo/open-source-hardware) programmed with [ESPHome](https://esphome.io/).
- The supply line temperature adjustment is based on ESPHome PID control[^1][^2].
- The [mixing valve actuator](https://esbe.eu/group/products/rotary-actuators/ara600-3-point) is an electric motor operated with 24 VAC.
- The switching modules are equipped with [solid state relays](https://www.velleman.eu/products/view/?id=461412).
- The controller can operate in automatic, semi-automatic or manual mode. Automatic mode with outdoor temperature compensated supply line temperature control. Semi-automatic mode with thermostatic supply line temperature set-point. Manual mode with static mixing valve position and manual increase/decrease.
- Line temperature sensors are digital [DS18B20](https://www.energibutiken.se/sv/dallas-1-wire-givare/10-dallas-1-wire-pro-rorgivare-02006.html) sensors.
- Outdoor temperature sensor is either an analog NTC (UVEAB VR-U) or a digital [DS18B20](https://www.energibutiken.se/sv/dallas-1-wire-givare/24-dallas-1-wire-pro-utegivare-02002.html).
- Integration with [Home Assistant](https://www.home-assistant.io/) is recommended. Sensor data and configuration parameters can be imported to operate the controller.

[ESP32 SSR Floor Heating Controller](https://github.com/jnasholm/fhc-pwm)

## Description

t.b.w.

**Note:** The controller hardware is theoretically capable of operating 110-230 VAC mixing valve actuators. This will however not be tested in the project.

[^1]: [PI Parameter Influence on Underfloor Heating Energy Consumption and Setpoint Tracking in nZEBs](https://www.mdpi.com/1996-1073/13/8/2068)
[^2]: [Reglering, om P-, I-, D-bidraget](https://www.bastec.se/anvandarmanual/reglering-p-i-d-bidraget/)
