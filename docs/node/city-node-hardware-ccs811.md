# CCS811

## Indoor air quality
[ref](https://www.idt.com/eu/en/document/whp/overview-tvoc-and-indoor-air-quality)
* Conversion from mg/m3to ppm for most common TVOC is by the factor ca. 0.5; for example, 10mg/m3equals ca. 5ppm.
* Conversion from ppm to ppb is by the factor 1000; for example, 0.1ppm equals 100ppb.

## Technical specs
equivalent CO2 (eCO2) and Total Volatile Organic Compound (TVOC) sensor.

* [Datasheet](https://cdn.sparkfun.com/assets/learn_tutorials/1/4/3/CCS811_Datasheet-DS000459.pdf)
* [Adafruit library](https://github.com/adafruit/Adafruit_CCS811/)

### Sense:  
* eCO2 from 400 to 8192 ppm
* TCOV from 0 to 1187 ppb

### Work environement
* Humidity
	* Storage: 0 %r.h to 90 %r.h
	* Operation: < 70 %r.h
* Temperature
	* Storage: -40 °C to 125 °C
	* Operation: -5 °C to 50 °C

### Power:
* Operation voltage: 1.8 to 3.3 V 
* Current
	* Active: 26 mA (46 mW)
	* Sleep: 19 uA (34 uW)

### Usage Concideration
* A new sensor must be burn-in for 48h to become stable
* After every long power off, the sensor must be run for 20 minutes before stable results are produced.

## HW connection on proto
sensor -> MCU
* VDD -> 3.3V
* GND -> GND
* SCL -> SCL (D19)
* SDA -> SDA (D18)
* ADDR -> GND
* nRESET -> NC
* nINT -> NC
* nWAKE -> GND -> always listing the I2C bus