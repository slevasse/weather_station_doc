# Setup the Pi

## Pi maintenance
```shell
sudo apt-get update
sudo apt-get upgrade
sudo reboot
```

## Headless SSH connection
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

## Change the default password and create a new user
* Once connected over ssh, open the software config tool `sudo raspi-config`
* Change the password.
* Also change the boot option to make sure you autolog with the pi user

## Install dependencies
### python (build latest version)

```shell
apt install libffi-dev libbz2-dev liblzma-dev libsqlite3-dev libncurses5-dev libgdbm-dev zlib1g-dev libreadline-dev libssl-dev tk-dev build-essential libncursesw5-dev libc6-dev openssl
wget https://www.python.org/ftp/python/3.8.2/Python-3.8.2.tar.xz
tar xf Python-3.8.2.tar.xz
rm Python-3.8.2.tar.xz
cd Python-3.8.2
./configure --enable-optimizations
make -j -l 4
sudo make altinstall
sudo nano ~/.bashrc
alias python='python3.8'
```
### PIP
```shell
curl -O https://bootstrap.pypa.io/get-pip.py
sudo python3.6 get-pip.py
```

### Git
* Install `sudo apt install -y git`
* Setup ssh key
```shell
mkdir -p $HOME/.ssh 
chmod 0700 $HOME/.ssh
ssh-keygen
cat ~/.ssh/id_rsa.pub
```
* copy that line and add to github


### other python dep
#### Numpy
`pip install numpy`
`sudo nano ~/.bashrc`
`/home/pi/.local/bin`

#### h5py
``` shell
sudo pip install cython
sudo apt-get install libhdf5-dev
sudo pip install h5py
```
* A note on `libhdf5-dev` it is a version that can support parallel IO. So I should be able to write to a hdf5 file while at the "same" time read with the webb server.. [source](https://packages.debian.org/buster/libhdf5-dev)

#### Paho
`sudo pip install paho-mqtt`

## Setup the Mosquitto brocker
The mosquitto broker is light weight and simple to setup. -> Good for my rpi4.
[source](https://randomnerdtutorials.com/how-to-install-mosquitto-broker-on-raspberry-pi/)

* Install `sudo apt install -y mosquitto mosquitto-clients` 
* enable at boot `sudo systemctl enable mosquitto.service`

## Mosquitto MQTT server
* Reboot the server `sudo service mosquitto restart`


## Setup to have a static IP address
[source](https://thepihut.com/blogs/raspberry-pi-tutorials/how-to-give-your-raspberry-pi-a-static-ip-address-update)
* I used .43. because it is on my phone router, probably need to change that when hooking to my home router.
```shell
sudo nano /etc/dhcpcd.conf

interface wlan0

static ip_address=192.168.43.243
static routers=192.168.43.1
static domain_name_servers=192.168.43.1
```

## Install the LAMP server
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

### Test the install
1. Type `hostname -I` in a terminal of the pi to get its IP address
2. In a webbrowser type connected to the local network type the IP you got from the previous step e.g.`192.168.43.243`

## Install Wordpress
[source](https://samhobbs.co.uk/2014/02/how-to-install-wordpress-on-a-raspberry-pi)
```shell
wget http://wordpress.org/latest.tar.gz
tar -xzvf latest.tar.gz
cd wordpress
sudo cp -r * /var/www/
```