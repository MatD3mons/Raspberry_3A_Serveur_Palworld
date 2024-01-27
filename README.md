# Raspberry_3A_Serveur_Palworld

install Raspbian 64 bit.

# a fresh start, so check for updates
sudo apt-get update && sudo apt-get upgrade

# add SWAP
sudo nano /sbin/dphys-swapfile
sudo nano /etc/dphys-swapfile
sudo reboot

# install screen
sudo apt install screen
screen -r palworld

# curl
sudo apt install curl

# install box64
sudo apt install git && sudo apt install cmake
git clone --branch v0.2.4 https://github.com/ptitSeb/box64.git
cd ~/box64
mkdir build && cd build
cmake .. -DRPI4ARM64=1 -DCMAKE_BUILD_TYPE=RelWithDebInfo
sudo make install
sudo systemctl restart systemd-binfmt

