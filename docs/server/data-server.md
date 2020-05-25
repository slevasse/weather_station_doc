# Data Server

[Git repo](git@github.com:slevasse/mqtt2hdf5_server.git)

After so e research I decided to settle on using MQTT to transmit data from node to server.
The data is then put into a hdf5 database.
I will use the [Mosquitto broker](https://mosquitto.org/) as front end of my data server.
The broker will receive the data from the network.
A python backend will handle the hdf5 database.
The backend will connect as a client to the broker using the ["wild card subscription" method](https://iotbytes.wordpress.com/store-mqtt-data-from-sensors-into-sql-database/) to get the data sent from the nodes.


## Requirements
A list the requirements of the data server.

* Handle MQTT transactions.
	* Use some kind of user password security.
* Store data in a predefined hdf5 database.
* Voila !

## Setup the Mosquitto brocker
The mosquitto broker is light weight and simple to setup. -> Good for my rpi4.
[source](https://randomnerdtutorials.com/how-to-install-mosquitto-broker-on-raspberry-pi/)

* Install `sudo apt install -y mosquitto mosquitto-clients` 
* enable at boot `sudo systemctl enable mosquitto.service`

## Mosquitto MQTT server
* Reboot the server `sudo service mosquitto restart`


## Run the server at boot
[source](https://www.raspberrypi.org/documentation/linux/usage/rc-local.md)
* open the rc.local file `sudo nano /etc/rc.local`
* add this line `python3.8 /home/pi/mqtt2hdf5_server/python-backend/start_data_server.py &` before the `exit 0` statement
	* The `&` run the command in a new thread.. to avoid locking up the pi at boot
* rc.local run as root, make sure the command used in there run as root user (e.g. `sudo -i` then whatever command..)