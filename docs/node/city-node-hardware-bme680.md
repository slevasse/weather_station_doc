# BME680

## Technical specs
Particulate matter sensor.

* Sense:  
	* PM2.5 and PM10 from 0.0-999.9 μg/m3
	* Error @ 25 °C and 50 %r.h: Max 15 % and 10 μg/m3
	* __Unreliable for RH > 80% -->[paper](https://www.researchgate.net/publication/330544166_Performance_Assessment_of_a_Low-Cost_PM25_Sensor_for_a_near_Four-Month_Period_in_Oslo_Norway)__
* [Datasheet](https://cdn-reichelt.de/documents/datenblatt/X200/SDS011-DATASHEET.pdf)
* Library before tweek for samd21 arduino -> [link](https://github.com/Silvan85/Nova_SDS011)

### Work environement
* Air pressure: 86 kPa to 110 kPa
* Humidity
	* Storage: 0 %r.h to 90 %r.h
	* Operation: < 70 %r.h
* Temperature
	* Storage: -20 °C to 60 °C
	* Operation: -10 °C to 50 °C

### Power:
* Operation voltage: 5 V 
* Current
	* Active: 70 mA ± 10 mA (300 mW to 400mW (avg 350 mW))
	* Sleep: < 4 mA (< 20 mW)



## Library setup
* There is two option to run this guy from the arduino ide.
  * Use a third party library (e.g. [adafruit](https://github.com/adafruit/Adafruit_BME680)). This works fine but you lack all the advanced feature the bme680 can provide, better use the bme280...
  * use the [BSEC](https://www.bosch-sensortec.com/software-tools/software/bsec/) and go through pain but have nice measurements out of the sensor.

### Get the BSEC to work
After spending some time using the BSEC, I came to the conclusion that I do not want to use it.
The BSEC is meant to run a its own thing and you cannot really control what the bme680 is doing.
Moreover the hot plate sensor takes a good 30 minute to be stable and have reliable measurement, which is incompatible with the very low datarate my system is running at.
(I cannot wait 30 minute every 20 minute the get a measurement..)
The main problem being that the BSEC has a fix polling interval while I would prefer to run the sensor in the forced mode (one measurement and sleep..)

* Download the library from [here](https://www.bosch-sensortec.com/software-tools/software/bsec/).
* Install the BSEC and Bme680_Data libraries manualy in the arduino ide.
* AND USE THE IDE IN VERSION 1.8.11 (as of 24/05/2020) the version 1.8.12 is bad and refuse to compile the bsec library proprely..
* The default I2C address is `0x77`, the variable used in the bsec exemples is not set to the default address for some reason.. So make sure the address is 0x77
