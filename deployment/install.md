# install mongo (ubuntu 16.04)

# Import they key for the official MongoDB repository
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927

# Create list file for mongo
echo "deb http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list

sudo apt-get update

sudo apt-get install -y mongodb-org

sudo systemctl start mongod

sudo systemctl status mongod

# Enable automatically starting MongoDB when the system starts.
sudo systemctl enable mongod

# Node
sudo apt-get install nodejs
sudo apt-get install npm
# not sure why we need nodejs-legacy
sudo apt install nodejs-legacy

# Further reading
# Set up Node.js application for production - Ubuntu
https://www.godaddy.com/help/set-up-nodejs-application-for-production-ubuntu-17352

#Niginx Fix
https://www.digitalocean.com/community/tutorials/how-to-set-up-nginx-server-blocks-virtual-hosts-on-ubuntu-16-04
