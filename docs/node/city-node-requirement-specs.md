# Node Requirement and Specifications

These are given as a best effort based requirements. I will try to meet these but I will not stop the project if I fail to meet some.

* The concept of a node is defined as a simple "smart" environemental sensor.
* The node is connected to the server via a network (ethernet, bluetooth, LoRa..) to transmit its measurements.
	* In case of total or transcient absence of network connectivity, the measurements are logged onboard the node for later retransmition or manual recovery.
* It can be either power autonomous (portable / remote area) or power dependant (places were power can be provided).
* To guaranty a wide spread deployement, the node shall be as cheap as possible, while maintaining a somewhat reliable system (no janky HW recup).
* The end goal is to have an autonomous sensor that can be used in hard environement (e.g. high montain). 

## Measurement types
The idea is to have a basic node that provide base measurements and a large panel of aditional measurements types to fit most users need.

* All measurements shall be timestamped (localy with GPS time standard).

### Base measurements
The node shall measure the folowing environemental parameters:

* Air temperature in Kelvin (K)
* Air pressure in Pascal (Pa)
* Relative humidity in %

### Air measurements
* wind speed
* wind direction
* Particulate mater concentration in air
	* PM2.5 (2.5 um and smaller)
	* PM10 (10 um and smaller)
* Total volatile organic compound (Tvoc)
* Carbon dioxide (CO2) 
* Ozone (O3)
* nitrogen dioxide (NO2)
* Caron monoxide (CO)
* Other gasses such as propane, butane etc... (for a potential kitchen monitoring system..)
* Radon 

### Cosmic measurements
* cosmic muons ? (just for fun)

### Ground measurements
* Ground temperature
* Ground humidity

## Mechanical & environemental
### MkI
* The size of the basic city node shall be within 90 x 60 x 30 mm3 (bit bigger than a [RPi4](https://www.raspberrypi.org/products/raspberry-pi-4-model-b/specifications/))
* The node shall be IP53 compliant
	* Somewhat Dust tight
	* Rain proof
* Mild shock/impact resistance (in case of accidental fall).
* Operational temperature range -20C to +60C.
* Operational rel humidity 0% to 85%. 
* Not suited for marine environement
* Mild UV resistance

### MkII
* Maximum size 200 x 200 x 200 mm3
* IP67 compliant
	* Dust tight
	* Water tight
* Proper shock/impact resistance (in case of accidental fall in the montain).
* Ice proof
* Operational temperature range -40C to +60C.
* Operational rel humidity 0% to 85%. 
* Altitude up to 5km
* Potential use close to shore 
* Strong UV resistance

## Operational
### MkI
* Polling period 1 to 1440 min (24h)
* Low power in between measurements
* Power shall be provided from USB or other low voltage sources.
	* A battery powered version shall work for 1yr continuously.
* All measurements are timestamped
* Capacity to keep track of time even in case of loss of power (RTC)
* Capacity to store up to 1yr of data onboard

### MKII
Same as MKI plus:

* Battery powered for up to 3 months
* Solar powered
* System vitals are monitored along with the other environmental measurements 

## User interface
### MkI
* On/off switch
* Some leds to give the state of the system
* Optional screen to give a live reading of the values
* Wake-up / force measurement push button (with a rate of max 0.1Hz)

### MkII
* on/off switch
* Some leds to give the state of the system
* Wake-up / force measurement push button (with a rate of max 0.1Hz)