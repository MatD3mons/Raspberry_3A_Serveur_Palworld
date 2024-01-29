base on : https://pimylifeup.com/raspberry-pi-palworld-server/ 

take 5 h minimum xD

# Raspberry_3A_Serveur_Palworld

install Raspbian 64 bit.

// try on ubuntu 22.04 64 bits

# a fresh start, so check for updates
```
sudo apt-get update && sudo apt-get upgrade
```
# add SWAP
```
// sudo apt-get install dphys-swapfile
sudo nano /sbin/dphys-swapfile
sudo nano /etc/dphys-swapfile
sudo reboot
```
# install screen
```
sudo apt install screen
screen -r palworld
```
# curl
```
sudo apt install curl
```

# git and cmake
```
sudo apt install git && sudo apt install cmake
```

# add
```
sudo dpkg --add-architecture armhf
sudo apt update
sudo apt install gcc-arm-linux-gnueabihf libc6:armhf libncurses5:armhf libstdc++6:armhf
```

# install box86
```
git clone https://github.com/ptitSeb/box86
cd ~/box86 && mkdir build && cd build
cmake .. -DRPI3ARM64=1 -DCMAKE_BUILD_TYPE=RelWithDebInfo
sudo make -j$(nproc)
sudo make install
```

# install box64
```
git clone https://github.com/ptitSeb/box64.git
cd ~/box64 && mkdir build && cd build
cmake .. -DRPI3ARM64=1 -DCMAKE_BUILD_TYPE=RelWithDebInfo

sudo make -j$(nproc)
sudo make install
```

# restart and reboot

```
apt-get install lib32gcc1
sudo systemctl restart systemd-binfmt
reboot
```

# Palworld server
-m: Automatically creates the user's home directory.
```
screen -R palworld
sudo useradd palworld -m 
//passwd -d palworld
```

```
cd /home/
sudo -u palworld -s
cd /palworld
mkdir ~/steamcmd && cd ~/steamcmd
curl -sqL "https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz" | tar zxvf -
./steamcmd.sh
quit
```

# Getting the Steamworks SDK Redistributable for Palworld
```
mkdir -p ~/.steam/sdk64/
./steamcmd.sh +login anonymous +app_update 1007 +quit
cp ~/Steam/steamapps/common/Steamworks\ SDK\ Redist/linux64/steamclient.so ~/.steam/sdk64/
```
# Installing the Palworld Dedicated Server on a Raspberry Pi
```
./steamcmd.sh +login anonymous +app_update 2394010 validate +quit
```

# Edit config
```
cp ~/palworldserver/DefaultPalWorldSettings.ini ~/palworldserver/Pal/Saved/Config/LinuxServer/PalWorldSettings.ini
nano ~/palworldserver/Pal/Saved/Config/LinuxServer/PalWorldSettings.ini
```

# Run Palworld server
```
cd ~/Steam/steamapps/common/PalServer
./PalServer.sh
```

# Open port
```
sudo apt-get install ufw
sudo ufw allow 8211
```

# Connecting to your Raspberry Pi Palworld Server
connect to 192.168.1.11:8211
