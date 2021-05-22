# esphome-iSpindel
iSpindel for ESPHome

Inspired by the awesome project iSpindel: https://www.ispindel.de/

This ESPHome sketch will let you setup the iSpindel for integrating it seamlessly into Home Assistant via ESPHome.


## Hardware
* Wemos D1 Mini
* MPU6050 Accelerometer/Gyroscope Sensor
* TP4056 Standalone Linear Li-lon Battery Charger
* 18650 Battery
* a Petling
* some balancing weights (5g) for calibration

## How it works
All the magic happens by measuring the current angle of the MPU6050. In my setup this is the angle of y-axis. With the initial calibration described in detail on https://www.ispindel.de/ the angle lets you read the current gravity of the wort and by comparing it to the initial gravity of course also the current share of alcohol. To have current gravity and alcohol in the Lovelace UI I created some template sensors where also the calibration parameters are used. For the original gravity input I use a numeric input helper `input_number.og_spindel_1`.

## Some more functionalities in the sketch
To monitor the current battery state I read the voltage on A0 and calculate as absolute and percentage voltage in relation to the parameters of the 18650 battery. It results in two additional sensors, the percentage one as `device_class: battery` to use it in HA.
For power saving the iSpindel goes to deep-sleep after 20 seconds of sending values for 30 mins. When not switching it on and not comming from deep-sleep the iSpindel waits 3 mins before going to deep-sleep for the first time. This gives you time for mainainance. It also looks for a boolean helper `input_boolean.esphome_prevent_deep_sleep` that you can define in Home Assistant. With this set to true the iSpindel will not go to deep-sleep at all. Keep in mind that you need to connect D0 and RST on a ESP8266 board to be able to use deep-sleep.
