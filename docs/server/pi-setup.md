# Setup the Pi

### Pi maintenance
```shell
sudo apt-get update
sudo apt-get upgrade
sudo reboot
```

### Headless SSH connection
[Source](https://howchoo.com/g/ndy1zte2yjn/how-to-set-up-wifi-on-your-raspberry-pi-without-ethernet)

1. In the OS SD card
2. Go to boot `cd /Volumes/boot`
1. (Optional) Add a empty file called ssh `touch ssh`
3. Add/edit a file called `wpa_supplicant.conf`
```shell
country=US # Your 2-digit country code
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
network={
   ssid="YOUR_NETWORK_NAME"
   psk="YOUR_PASSWORD"
   key_mgmt=WPA-PSK
}
```
4. Boot the Pi
5. Connect to the Pi via ssh `ssh -Y pi@raspberrypi`
6. The default password is `raspberrypi`

### Change the default password and create a new user
* Once connected over ssh, open the software config tool `sudo raspi-config`
* Change the password.

### Install the LAMP server
[Source](https://randomnerdtutorials.com/raspberry-pi-apache-mysql-php-lamp-server/)

1. `sudo apt update`
1. Install the server `sudo apt install apache2`
2. Install PhP `sudo apt install php -y`
3. Restart the apache server `sudo service apache2 restart`
3. install mariadb (mysql) `sudo apt install mariadb-server php-mysql -y`
3. Restart the apache server `sudo service apache2 restart`
4. Secure the sql database `sudo mysql_secure_installation`
4. Create a new user for the database 
```shell
pi@raspberrypi:/var/www/html $ sudo mysql --user=root --password
> create user admin@localhost identified by 'your_password';
> grant all privileges on *.* to admin@localhost;
> FLUSH PRIVILEGES;
> exit;
```
5. Install phpmyadmin `sudo apt install phpmyadmin -y`
6. enable mysqli `sudo phpenmod mysqli`
3. Restart the apache server `sudo service apache2 restart`
4. Move the files to the server folder `sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin`
5. login phpmyadmin @ `http://SERVERIP/phpmyadmin` , use the username: admin and the password you set before.

#### Test the install
1. Type `hostname -I` in a terminal of the pi to get its IP address
2. In a webbrowser type connected to the local network type the IP you got from the previous step e.g.`192.168.43.243`