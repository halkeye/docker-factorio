# Introduction
This repository was forked from [carloe/docker-factorio](https://hub.docker.com/r/carloe/docker-factorio/) to build a custom configured server and provide extra information on how to setup a server using docker.

# Server Setup

The easiest way to setup a server by far, is to use [Digital Ocean.](https://m.do.co/c/ebb875976e21) Follow the link to signup and get $10 for free to start you off. This guide assumes your are using a Mac, Windows users may have to alter their method when it comes to terminal usage. Disclaimer: All referrals generate credit to my account for more server goodness.

1. Sign up to [Digital Ocean](https://m.do.co/c/ebb875976e21)
2. Click 'Create droplet' button  
<img src="https://cloud.githubusercontent.com/assets/6523593/21289644/2160bb62-c49a-11e6-9a01-b9556445ffec.png" width="300">  
3. Under 'Choose an image' select 'One-click apps'  
<img src="https://cloud.githubusercontent.com/assets/6523593/21289645/2160d250-c49a-11e6-89f5-328e299d07a2.png" width="500">   
4. Select 'Docker 1.12.\* on 16.04'  
<img src="https://cloud.githubusercontent.com/assets/6523593/21289646/216100d6-c49a-11e6-95ad-5f6dd85ddc61.png" width="500">  
5. Select the '$5/mo' size, this is fine for a basic multiplayer server  
<img src="https://cloud.githubusercontent.com/assets/6523593/21289647/21620c92-c49a-11e6-838a-55c790590229.png" width="400">  
6. Select the datacenter that is closest to you  
<img src="https://cloud.githubusercontent.com/assets/6523593/21289642/2146c11c-c49a-11e6-9c00-05978a9d8bfa.png" width="500">  
7. Add your [SSH public key](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2). Alternatively, you will need to take note of the password that Digital Ocean emails to you after you setup the server.
8. You will only need 1 droplet, name your server and click create  
<img src="https://cloud.githubusercontent.com/assets/6523593/21289643/215f73ce-c49a-11e6-9803-0f45b226eced.png" width="200">  

# Connect to your server

Windows users will need to look at alterations for using PuTTY.

1. Make note of the IP address of your server (ipv4)  
<img src="https://cloud.githubusercontent.com/assets/6523593/21289648/2184ce1c-c49a-11e6-82a0-fdd6fa07a04f.png" width="500">  
2. Open terminal
3. `ssh root@IPaddress`, obviously replace 'IPaddress' with the IP address.

# Build Your Server

The latest Factorio headless server is downloaded at build time.

1. `git clone https://github.com/LpmRaven/factorio-docker-server.git`
2. `cd factorio-docker-server`
3. Config your server (see section below then return here)
4. `docker build -t factorio-instance-name .`

# Server Configuration

1. `open .`
2. Edit the map-gen-settings.json file and the server-settings.json file (DO NOT RENAME THE FILES)
3. Continue to read the previous section

# Persisting Saves

Use a docker volume to persist the savegames on the host machine rather than in the docker container.

```bash
mkdir $(pwd)/saves

# Make sure the saves dir can be written to by the "factorio" user in Docker, with uid 1000
sudo chown 1000:1000 $(pwd)/saves
# (alternatively, if you don't have root): chmod 777 $(pwd)/saves

```

# Load a Save/Map

To load a save, place the save file in the saves folder and name the file: factorio_save.zip
Remove all other files from the save folder as it will load the most recently updated file.

# Basic Usage

The init script will automatically create a new save game if none exists.

Then launch your container as usual.

```bash
docker run -d \
           -v $(pwd)/saves:/opt/factorio/saves \
           -p 34197:34197/udp \
           --restart=always \
           --name factorio-server \
           factorio-instance-name
```

# Play the game!

1. Open Factorio
2. Connect to your server `IPaddress:34197`, change IP address obviously.
3. Default password is `donationswelcome`, feel free to change it.
4. Play the game!

Enjoy. Hope this guide helped you setup your factorio server!
