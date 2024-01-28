base on : https://pimylifeup.com/raspberry-pi-palworld-server/ 

take 5 h minimum xD

# Raspberry_3A_Serveur_Palworld

install Raspbian 64 bit.

# a fresh start, so check for updates
```
sudo apt-get update && sudo apt-get upgrade
```
# add SWAP
```
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
# install box64
```
sudo apt install git && sudo apt install cmake
git clone --branch v0.2.4 https://github.com/ptitSeb/box64.git
cd ~/box64
mkdir build && cd build
cmake .. -DRPI4ARM64=1 -DCMAKE_BUILD_TYPE=RelWithDebInfo
sudo make install
sudo systemctl restart systemd-binfmt
reboot
```
# install box86
```
git clone https://github.com/ptitSeb/box86
sudo dpkg --add-architecture armhf
sudo apt install gcc-arm-linux-gnueabihf libc6:armhf libncurses5:armhf libstdc++6:armhf
cd ~/box86
mkdir build && cd build
cmake .. -DRPI4ARM64=1 -DCMAKE_BUILD_TYPE=RelWithDebInfo
sudo make install
sudo systemctl restart systemd-binfmt
reboot
```
# Palworld server
```
sudo useradd palworld -m # -m: Automatically creates the user's home directory.
exit
passwd -d $username
su palworld
cd /home/palworld
mkdir ~/steamcmd
cd ~/steamcmd
./steamcmd.sh
quit
```
# Getting the Steamworks SDK Redistributable for Palworld
```
./steamcmd.sh +force_install_dir ~/steamworkssdk +@sSteamCmdForcePlatformType linux +login anonymous +app_update 1007 validate +quit
mkdir -p ~/.steam/sdk64
cp ~/steamworkssdk/linux64/steamclient.so ~/.steam/sdk64/
```
# Installing the Palworld Dedicated Server on a Raspberry Pi
```
./steamcmd.sh +force_install_dir ~/palworldserver +@sSteamCmdForcePlatformType linux +login anonymous +app_update 2394010 validate +quit
cd ~/palworldserver/
./PalServer.sh
```
# Config
```
cp ~/palworldserver/DefaultPalWorldSettings.ini ~/palworldserver/Pal/Saved/Config/LinuxServer/PalWorldSettings.ini
nano ~/palworldserver/Pal/Saved/Config/LinuxServer/PalWorldSettings.ini
```
```
ServerPassword="" // change this minimum
```

```
exit
sudo nano /etc/systemd/system/palworld.service
```
```
[Unit]
Description=Palworld Server
Wants=network-online.target
After=network-online.target

[Service]
User=palworld
Group=palworld
WorkingDirectory=/home/palworld/
ExecStartPre=/home/palworld/steamcmd/steamcmd.sh +force_install_dir '/home/palworld/palworldserver' +login anonymous +app_update 2394010 +quit
ExecStart=/home/palworld/palworldserver/PalServer.sh -useperfthreads -NoAsyncLoadingThread -UseMultithreadForDS > /dev/null
Restart=always

[Install]
WantedBy=multi-user.target
```
```
sudo systemctl enable palworld
sudo systemctl start palworld
// sudo systemctl stop palworld
// sudo systemctl disable palworld
```
# Connecting to your Raspberry Pi Palworld Server
connect to 192.168.1.11:8211
