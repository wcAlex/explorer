Node / Iquidus Explorer Setup for Dummies
Pulse Crypto is used in this example.

This Tutorial is going to create a Daemon (node) and install Explorer.
THIS IS NOT GOING TO CREATE A GUI CLIENT.

Follow the instructions in [whatever coin name] docs folder Unix build - some builds are different.

I setup this up on both Ubuntu 15.10 and 16.04 with no issues.
You can create an account on vultr and get $50 free to be used in 2 months.
Non-refferal link:  https://www.vultr.com/freetrial/

******************************************
*** Change Password / Update System
******************************************

Change passwords
# sudo passwd  //change root password

Update Packages
# sudo apt-get update
# sudo apt-get upgrade -y

Install git and nano
# sudo apt-get install nano git -y

******************************************
*** Installing Pulse Node
******************************************

Get the Source Code

Basic packages to build the coin
# sudo apt-get install build-essential libssl-dev libdb++-dev libboost-all-dev libqrencode-dev miniupnpc libminiupnpc-dev autoconf pkg-config libtool autotools-dev libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev automake -y

Create a Swap File - Low Ram Servers
# sudo fallocate -l 4G /swapfile
# sudo chmod 600 /swapfile
# sudo mkswap /swapfile
# sudo swapon /swapfile
# sudo nano /etc/fstab

Add to fstab file
/swapfile   none    swap    sw    0   0

Adding the Coin
# sudo git clone https://github.com/elcrypto/Pulse.git

Change Directories to src/
# cd Pulse/src/ 

Compile the code with flags
# sudo make -f makefile.unix USE_UPNP=1
# sudo strip pulsed

Stat the file with daemon flag so that you can create a config file
# sudo ./pulsed -daemon

Create a conf file in .pulse/pulse.conf
# cd
# sudo nano .pulse/pulse.conf

rpcuser=pulserpc
rpcpassword=Gnfh67gdmRTGvA5YB3UZhV6e6cPTTeneTTdnosLLD3cU
listen=1
maxconnections=500

*make sure to set the file to read only.
# sudo chmod 400 .pulse/pulse.conf
# cd Pulse/src
# sudo ./pulsed -daemon -txindex

Check it daemon has started.  If it returns information, the daemon is working!
# sudo ./pulsed getinfo

Stopping the daemon
# sudo ./pulsed stop

******************************************
*** Setting up the Explorer
*** https://github.com/iquidus/explorer
******************************************

Install MongoDB Community Edition
https://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/

# sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
# echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
# sudo apt-get update
# sudo apt-get install -y mongodb-org

Default init system is systemd which was Upstart previously. So you need to install Upstart, reboot your system and here you go, you can now run mongodb service.

Install Upstart
# sudo apt-get install upstart-sysv -y

# Reboot your system
# sudo reboot
# sudo service mongod start

Running MongoDB - reference
# sudo service mongod start
# sudo service mongod stop
# sudo service mongod restart

Installing Nodejs
# sudo apt-get update
# sudo apt-get install nodejs nodejs-legacy -y
# sudo apt-get install npm -y

Creating a MongoDB Database
# sudo mongo
> use explorerdb
> db.createUser( { user: "3er44we222", pwd: "3eu4ue8hhk988!kd8", roles: [ "readWrite" ] } )
> exit

Installing the Explorer
# cd /home/
# git clone https://github.com/iquidus/explorer explorer
# cd explorer && npm install --production
# cp ./settings.json.template ./settings.json

Modify the Settings File
# sudo nano settings.json

See if it's working
# npm start

Update the databases
# sudo node scripts/sync.js index update 

Install Forever to keep the js running
# sudo npm install forever -g
# sudo npm install forever-monitor

Start the Explorer
# forever start bin/cluster

******************************************
*** Installing Cron
*** https://help.ubuntu.com/community/CronHowto
******************************************

# sudo apt-get install gnome-schedule -y

Editing Cron
# sudo crontab -e

Add Cron to File to update the explorer
*/1 * * * * cd /path/to/explorer && /usr/bin/node scripts/sync.js index update > /dev/null 2>&1

Add Cron to make sure the Pulsed runs (NOT WORKING)
If anyone has a good reboot for this, please post in the comments.
@reboot cd /root/Pulse/src ./pulsed -daemon -txindex
@reboot cd /root/explorer forever start bin/cluster 
