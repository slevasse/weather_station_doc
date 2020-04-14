# Node Requirement and Specifications

* The concept of a node is defined as a simple "smart" environemental sensor.
* The end goal is to have an autonomous sensor that can be used in hard environement (e.g. high montain). 
* The node is connected to the server via a network (ethernet, bluetooth, LoRa..) to transmit its measurements.
	* In case of total or transcient absence of network connectivity, the measurements are logged onboard the node for later retransmition or manual recovery.
* It can be either power autonomous (portable / remote area) or power dependant (places were power can be provided).
* To guaranty a wide spread deployement, the node shall be as cheap as possible, while maintaining a somewhat reliable system (no janky HW recup).

## Measurement types
The concept is to have a basic node that provide base measurements and a large panel of aditional measurements types to fit most user needs.

### Base node
The base node shall measure the folowing environemental parameters:

* Air temperature in Kelvin (K)
* Air pressure in Pascal (Pa)
* Relative humidity in %

### Air node
* wind speed
* wind direction
* Particulate mater concentration in air
	* PM2.5 (2.5 um and smaller)
	* PM10 (10 um and smaller)
* Total volatile organic compound (Tvoc)
* equivalent CO2 (eCO2) 
* Ozone (O3)
* nitrogen dioxide (NO2)
* Caron monoxide (CO)
* Radon 
* cosmic muons ? (just for fun)

### Ground node
* Ground temperature
* Ground humidity



All measurements shall be timestamped (localy with GPS time standard).

