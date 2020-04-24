# SDS1011

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



## Library
I wanted a complete library, however I could not find one that would work natively for the arduino nano IOT.
Therefore, I took the one from Radoslaw Orecki and modified it a bit.
I simply modified the constructor and removed the `begin` method from the class.
Also I commented the `#include <SoftwareSerial.h>` in the .h file. 
Instead of forking the project I just copied it and directly use the modified files in my project.

New constructor:
```CPP
// .h file
SDS011SW(Stream* serial);

// .cpp file

SDS011SW::SDS011SW(Stream* serial){
  _sdsSerial = serial;
}
```

Exemple of usage:
```CPP
#include "sds1011_sw.h"

// instanciate the object
SDS011SW sds011(&Serial1);
void setup()
{
  Serial.begin(9600); // Output to Serial at 9600 baud
  // give some time for the serial monitor to pop
  delay(5000);
  Serial.println("START");
  // start the uart
  Serial1.begin(9600);

  // Set the device id to 0xFFFF (optional)
  //sds011.setDeviceID(0xFFFF);

  // Set to query mode
  sds011.setDataReportingMode(DataReportingMode::query);

  // Set to continuous working
  sds011.setDutyCycle(0);

  //Set the device to sleep
  sds011.setWorkingMode(WorkingMode::sleep);
}

void loop()
{
  Serial.println("Loop");
  float p25, p10;
  // turn on the sensor
  sds011.setWorkingMode(WorkingMode::work);
  //wait for 30 second
  delay(30000);
  // query data
  QuerryError error_type = sds011.queryData(p25, p10);
  if (error_type == QuerryError::no_error)
  {
    Serial.println("no error");
    Serial.println(String(millis() / 1000) + "s:PM2.5=" + String(p25) + ", PM10=" + String(p10));
  }else if(error_type == QuerryError::no_new_data){
    Serial.println("No new data");
    Serial.println(String(millis() / 1000) + "s:PM2.5=" + String(p25) + ", PM10=" + String(p10));
  }else if(error_type == QuerryError::response_error){
    Serial.println("response_error");
    Serial.println(String(millis() / 1000) + "s:PM2.5=" + String(p25) + ", PM10=" + String(p10));
  }else if(error_type == QuerryError::call_to_often){
    Serial.println("call_to_often");
    Serial.println(String(millis() / 1000) + "s:PM2.5=" + String(p25) + ", PM10=" + String(p10));
  }
  // turn off the sensor
  sds011.setWorkingMode(WorkingMode::sleep);
  // wait 30 seconds
  delay(30000);
}
```

Output from the serial monitor
```
17:20:08.106 -> START
17:20:11.157 -> Loop
17:20:41.322 -> no error
17:20:41.322 -> 38s:PM2.5=2.40, PM10=4.80
17:21:11.430 -> Loop
17:21:41.671 -> no error
17:21:41.671 -> 98s:PM2.5=2.30, PM10=4.20
17:22:11.763 -> Loop
17:22:42.007 -> no error
17:22:42.007 -> 158s:PM2.5=2.10, PM10=4.40
17:23:14.060 -> Loop
17:23:44.845 -> no error
17:23:44.845 -> 219s:PM2.5=2.40, PM10=4.30

```
