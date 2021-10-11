* [Goals of Rpi](#goals-of-rpi)
* [1. My hardware setup](#1-my-hardware-setup)
* [2. Installation](#2-installation)
	* [2.1. Flashing your SD card with raspbian buster lite](#21-flashing-your-sd-card-with-raspbian-buster-lite)
		* [2.1.1. Setup SSH and wifi](#211-setup-ssh-and-wifi)
		* [2.1.2. Allowing login through SSH (without using a password)](#212-allowing-login-through-ssh-(without-using-a-password))
			* [2.1.2.1. Troubleshooting the Mac and SSH](#2121-troubleshooting-the-mac-and-ssh)
	* [2.2. Mmanaging USB drives](#22-mmanaging-usb-drives)
	* [2.3. OMV5 (openmediavault) on debian -- (FAILED!)](#23-omv5-(openmediavault)-on-debian----(failed!))
	* [2.4. AFP (netatalk / apple file protocol) on Pi (without docker) -- (WORKS!)](#24-afp-(netatalk-/-apple-file-protocol)-on-pi-(without-docker)----(works!))
	* [2.5. SMB (Samba) on Pi (without docker) -- (WORKS!!)](#25-smb-(samba)-on-pi-(without-docker)----(works!!))
	* [2.6. Docker for Raspbian -- (WORKS!)](#26-docker-for-raspbian----(works!))
	* [2.7. optional, moving the storage of your docker containers to external storage](#27-optional,-moving-the-storage-of-your-docker-containers-to-external-storage)
	* [2.8. Samba, using docker (WORKS!)](#28-samba,-using-docker-(works!))
	* [2.9. AFP (time machine share), using docker (WORKS!)](#29-afp-(time-machine-share),-using-docker-(works!))
	* [2.10. Homebridge on docker with docker-compose (WORKS!)](#210-homebridge-on-docker-with-docker-compose-(works!))
	* [2.11. Sonoshttpapi on docker with docker-compose (WORKS!)](#211-sonoshttpapi-on-docker-with-docker-compose-(works!))
	* [2.12. Music streaming server with (Docker) DAAPD](#212-music-streaming-server-with-(docker)-daapd)
* [3. useful tools and links](#3-useful-tools-and-links)
	* [3.1. on using docker](#31-on-using-docker)
		* [3.1.1. Running a docker container 'detached'](#311-running-a-docker-container-'detached')
		* [3.1.2. Shutting down your container](#312-shutting-down-your-container)
		* [3.1.3. Log in to the docker container](#313-log-in-to-the-docker-container)
		* [3.1.4. Update all your docker images](#314-update-all-your-docker-images)
		* [3.1.5. Connect a finder folder to SSH / SFTP (connect from your mac to the Pi)](#315-connect-a-finder-folder-to-ssh-/-sftp-(connect-from-your-mac-to-the-pi))

## Goals of Rpi

I had a lot of things going on on my iMac workstation that I wanted to move away to the Pi. The reasons behind this:

1. Leaving it on costs more power (for sake of mathematics, lets say 100W on average for the iMac, and 10W for the Pi)
2. I'm doing a lot of development and 'fun' on the iMac, which makes it prone to crash and/or having to reinstall. Having more people in the house suddenly not being able to turn lights on or off, is a terrible experience.
3. It needs to (keep) work(ing). Think about time machine backups, streaming content from an external drive, that shouldn't be bothering your other tasks. Even though a mac is unix underneath, a headless machine has proven to be more stable.
4. (shameful but) I didn’t use docker before! (and yes I know there’s K8s and stuff - that’s beyond the point)
5. Running this thing headless (allows it to hide in a cabinet or a spot thats not visible) is a nice challenge that I regretted a few times, when dealing with partitioning for instance.
6. Being independant from sonos, Apple or others to be able to stream audio throughout the house (and reducing cost) - I’m in love with Roon, but Roon core needs a heavier machine than a Pi. There _must_ be something that can replace that experience around! (current status: Roon server on mac, several Roon endpoints on Pi)

How could I forget how terrible I am at these things when I started...

I started over at least 5 times (flashing the image, rebooting the system, forgetting the WLAN config etc.) because I'm the stubborn one that wants to run this thing headless **only**...

I've learned, I'm sharing. There are a million websites and documents that share this process, but I'd like to do this with you, hoping it got you thinking in the right direction. If yes, you're welcome to leave a comment!

And came 2021 - I’m running 4 Pi’s at home!

## 1. My hardware setup

![my Apple Pi 🥧 accessed from the iPad](https://casey.berlin/wp-content/uploads/2021/03/2021-03-05-23.15.46.jpeg){.right}

- Rasp pi 4, 2 GB (wasn't any other in stock, happy camper, with ~~8-9~~15 containers running it sits quiet at about 50% memory )
- 64 GB SD card (pretty standard sandisk thingy, didn't bother to run it on the external drive - [performance is quite similar unless you run an SSD](https://www.tomshardware.com/news/raspberry-pi-4-ssd-test,39811.html)). I’ve ordered a USB to NVM2 converter to see the performance. That should be quite dramatic.
- 10 TB seagate drive as external storage
- Cabled connections, the wifi is not great.
- Mac OSX 11 for flashing using [Etcher](https://www.balena.io/etcher/) and SSH using [iTerm2](https://www.iterm2.com/)/ohmyZSH.

## 2. Installation

### 2.1. Flashing your SD card with raspbian buster lite

Picked lite because I have 2 GB of memory to work with, but makes this thing harder to do - we like the challenge don't we!

- Download buster lite (or desktop if you prefer) from [the standard place](https://www.raspberrypi.org/downloads/raspbian/)
- Download [Etcher](https://www.balena.io/etcher/)
- Flash your drive using Etcher
- Eject the disk
- Wonder why the bloody thing isn't doing anything.

#### 2.1.1. Setup SSH and wifi

Ah yes! Don’t forgot this!

- Create a new empty file called `SSH`. (no extensions or anything, plain 'SSH'. No content, this allows SSH access). copy it to the SD.
- Create a new file called `wpa_supplicant.conf` on the root of the SSD.

Contents of _wpa_supplicant.conf_:

```shell
country=de
update_config=1
ctrl_interface=/var/run/wpa_supplicant

network={
 ssid="<your SSID>"
 psk="<Password for your WiFi>"
}
```

- Change your country, SSD and PSK fitting your setup
- Copy both files to the root of your SD card (called BOOT now since flashing)
- Eject the drive
- Put it in the pi
- Cross your fingers again.
- Hope it works! (at this time I connected a display to be able to see what was happening, useful!)

_Note_: don't panic if you lose the SSH/wpa_supplicant files once your Pi has booted, that is expected behavior I found out. (by trial and error)

Go do the raspi-config thing now! `sudo raspi-config`

Change at least:

1. Your password
2. Your timezone
3. Hostname
4. Your password (not kidding!)

And don't forget to update your Pi regularly. Debian has an active community, your Pi uses a package system called "apt-get" that helps you keep your system updated.

That means that as soon as you've booted your pi again do the following:

```shell
sudo apt-get update
sudo apt-get upgrade
```

This will take time. Be happy! (and I got used to the wait, since I screwed up a lot and had to begin from start again)

#### 2.1.2. Allowing login through SSH (without using a password)

I hate entering passwords, especially if my device already asks for one. I enjoy the ease of 1password, and in case of 'security' of my Pies, SSH works well. It takes time to get your head around public/private keys and what they do, but you’ll manage.

For the purpose of logging into your Pi using SSH; do follow what Rpi already has written [down on their site](https://www.raspberrypi.org/documentation/remote-access/ssh/passwordless.md).

In short;

1. Generate a key on your device (where you want to log in *from*); `ssh-keygen` (in my case my mac)
2. Copy the generated key to your RPi - `ssh-copy-id <username>@<ip-address>' (e.g. pi@192.168.1.1)
3. try and log in, `ssh pi@192.168.1.1` - you might need to 'trust' it once, but then you're done!

**Optional but useful**
4. Add your passphrase to the mac keychain, saves you typing a PW each time; `ssh-add -K ~/.ssh/id_rsa`

##### 2.1.2.1. Troubleshooting the Mac and SSH

1. Make sure you setup the rights correctly on your pi's `~/.ssh` folder; (I changed the rights on my home folder because of reasons, but with that killed an easy login)

```shell
chmod 700 /home/your_user
chmod 700 /home/your_user/.ssh
chmod 600 /home/your_user/.ssh/authorized_keys
```

2. Apparantly the SSH Agent isn't always running correctly on the Mac. Make sure you add the following to your `~/.ssh/config` file;

```shell
Host *
    UseKeychain	yes
    AddKeysToAgent	yes
```

### 2.2. Managing USB drives

I formatted my 10 TB drive into a few pieces;

1. a 8 TiB partition for an Apple time machine
2. a 1 TiB partition for docker images / containers
3. The rest for SMB storage among other things.

All are EXT4 for now, the following command worked to format the drives:

```shell
sudo mkfs.ext4 /dev/sda1
```

Where SDA1 is the specific drive to format.

I'm skipping a few details here, since my scenario is specific. But make sure to read the following, since that gets it all together:

1. [How to format and mount a drive](https://raspberrytips.com/format-mount-usb-drive/)

I've considered changing the size a few times afterwards (my partner had a bigger need than I expected for disk space) - I decided to connect my drive to my mac, boot a virtual linux setup with a gui and use [Gparted](https://gparted.org/) to do so.

There is a commandline alternative where it’s based on called [parted](https://www.tecmint.com/parted-command-to-create-resize-rescue-linux-disk-partitions/), and in combination with fdisk can do destructive stuff. I didn’t touch that after one attempt that failed.

### 2.3. OMV5 (openmediavault) on debian -- (FAILED!)

We all fail. And this thing failed on my end. I thought I found a 'one way ticket' into a good solution ([OMV](https://www.openmediavault.org) offers it’s own docker management for instance), but this worked me into more problems than I thought. I felt this whole thing was much more 'bloatware' than what my purpose was here (lean and mean Pi machine!)

OMV allows you to work the other way around and that it will be your new "OS" to work with; it allows you to:

- Manage your filesystem
- Manage your drives/mounts
- Setup docker/docker images _inside_ OMV, you have it all done inside a GUI (using Portainer if I'm not mistaken)

*Status 25/3/20*:
It seems things are better now, but as said - YMMV...

- [Beta OMV5 installation instructions here](https://github.com/OpenMediaVault-Plugin-Developers/docs/blob/master/Adden-B-Installing_OMV5_on_an%20R-PI.pdf)

### 2.4. AFP (netatalk / apple file protocol) on Pi (without docker) -- (WORKS!)

This was my first attempt on getting AFP (apple's file protocol) going on the Pi. I had to do a full clean install to get rid of OMV5 here ^^ but hell, it worked!

AFP is a _very_ interesting powerful protocol that's much faster in comparison to SMB, but eventually I let go of AFP -- the docker images are not that great, I couldn't get them going the way I wanted (and AFP outside docker was pretty heavy on resources for some reason)

Read up if you want to go this route though ([and sponsor me a 8 GB version too](https://www.buymeacoffee.com/caseyberlin)!)

- [Setting up AFP on pi - Great resource!](https://pimylifeup.com/raspberry-pi-afp/)
- [What AFP.conf is all about](http://netatalk.sourceforge.net/3.1/htmldocs/afp.conf.5.html)

### 2.5. SMB (Samba) on Pi (without docker) -- (WORKS!!)

This worked pretty quickly and easily! Read below!

- [The easiest one to get going](https://www.raspberrypi.org/magpi/samba-file-server/)

_note_: Check the comments for buster commands

### 2.6. Docker for Raspbian -- (WORKS!)

Here the fun started. I started using docker on the Pi. My previous assumptions (command line virtual machines like vagrant / virtualbox) have always been a tough thing to get get my head around, and impossible to get rid of - I was hesitant to start.

But now I'm a happy docker camper; it runs smoothly -- even though some containers are heavier on resources than others. Be aware of that!

![My docker containers running smoothly together on the pi](https://casey.berlin/wp-content/uploads/2021/03/2021-03-05-23.17.43.jpeg)

1. Install docker

    ```shell
    apt-get update
    apt-get upgrade
    curl -fsSL get.docker.com -o get-docker.sh && sh get-docker.sh
    sudo apt-get -y install docker-compose
    ```

2. Make sure your user can execute docker;

    ```shell
    sudo usermod -aG docker pi
    ```

3. and see if it works now!

    ```shell
    docker run hello-world
    ```

4. Good! Ready to get docker-compose going (trust me on this one, it makes your typing life much easier)

    ```shell
    sudo apt-get install libffi-dev
    sudo apt-get install -y python python-pip
    sudo pip install docker-compose
    ```

### 2.7. optional, moving the storage of your docker containers to external storage

1. Moving the storage for docker files around wasn't that hard - I  followed the following by changing the daemon settings for Docker:

    ```shell
    sudo nano /etc/docker/daemon.json
    ```

2. Adding this content to that file (make sure this is the volume you want your containers on)

    ```json
    {
        "graph": "/mnt/docker"
    }
    ```

3. Reboot your Pi. (there are other ways, but I like `sudo reboot`, the power of the terminal!)

4. Check if docker is running from the right drive;

    ```shell
    docker info|grep "Docker Root Dir"
    ```

    It should tell you where it is storing it's stuff now.

5. Then you can safely remove the original docker setup (if any) on your SD card;

    ```shell
    rm -rf /var/lib/docker
    ```

### 2.8. Samba, using docker (WORKS!)

Super simple and quick to setup - It's far from secure, but that isn’t the purpose here (doing SMB on a Pi in a corporate environment is a bad idea, the performance isn't that great, but I’m at home)

- [Rpi Samba](https://hub.docker.com/r/trnape/rpi-samba/)

Docker command to run (still need to wrap into docker-compose):
```shell
docker run -d --restart=always \
-p 192.168.86.224:445:445 \
-v /home/pi/smb/conf:/etc/samba \
-v /home/pi:/share/pi_home \
-v /mnt/share:/share/share \
--name smb trnape/rpi-samba \
-u "maxmustermann:password" \
-s "Share:/share/share:rw:maxmustermann"
```

Rename your stuff accordingly, this is just an example. 

### 2.9. AFP (time machine share), using docker (WORKS!)

This one worked to get a TM volume going, but didn't do _that much_ else. YMMV!

- [Simple docker command to get a machine started](https://hub.docker.com/r/odarriba/timemachine-rpi)

docker-compose yaml:
```yaml
version: "3"
services:
  timemachine:
    image: timemachine-rpi
    ports:
      - "548:548"
      - "636:636"
    deploy:
      restart_policy:
        condition: any
        max_attempts: 2
    volumes:
      - /mnt/timemachine:/timemachine
```

### 2.10. Homebridge on docker with docker-compose (WORKS!)

By the time I was ready to move over my homebridge to the Pi I hit the 10 new installs mark for the Pi. I could dream the contents of the files, and started writing this .MD doc to document my progress.

It was time to put this thing to work! Let's go with homebridge!

I found the 'magic' of docker-compose, where I was able to get to know YAML and it's 'spacious' peculiarities.

```shell
mkdir /home/pi/homebridge
cd /home/pi/homebridge
sudo nano docker-compose.yml
```

Here's the contents of my docker-compose file;

```yaml
version: '2'
services:
  homebridge:
    image: oznu/homebridge:latest
    restart: always
    network_mode: host
    volumes:
      - ./config:/homebridge
    environment:
      - PGID=1000
      - PUID=1000
      - HOMEBRIDGE_CONFIG_UI=1
      - HOMEBRIDGE_CONFIG_UI_PORT=8080
      - TZ=Europe/Berlin
      - HOMEBRIDGE_DEBUG=0
```

Save (CTRL+O) and exit (CTRL+X, both of these will be your friends for life) the file, type ```docker-compose up``` and once more cross your fingers!

If you've installed docker, this should work now! Once the machine is running, it should be approachable on 'http://yourhostname:8080'.

*Very useful*:

1. [github article by oznu](https://github.com/oznu/docker-homebridge/wiki/Homebridge-on-Raspberry-Pi)

### 2.11. Sonoshttpapi on docker with docker-compose (WORKS!)

I'm using docker-compose to get this thing going, feel free to grab the script ([or fork me on Github](https://github.com/Keesromkes/rpi-docker-node-sonos-http-api))

1. make sure you setup your filestructure first, make a new directory where you'd like to setup all this.

    ```shell
    mkdir clips
    mkdir settings
    mkdir cache
    mkdir presets
    curl https://raw.githubusercontent.com/jishi/node-sonos-http-api/master/presets/example.json > presets/example.json
    ```

2. now open your 'docker-compose.yaml' file

    ```shell
    sudo touch docker-compose.yaml
    sudo nano docker-compose.yaml
    ```

3. and add to it these contents;

    ```yaml
    version: '2'
    services:
      sonoshttpapi:
        image: jonmaddox/rpi-docker-node-sonos-http-api
        restart: always
        network_mode: host
        volumes:
          - ./settings:/app/settings
          - ./clips:/app/clips
          - ./cache:/app/cache
          - ./presets:/app/presets
    ```

4. And one more container is done! Get her going again with ```docker-compose up```

Very useful:

1. [github repo by @maddox](https://github.com/maddox/rpi-docker-node-sonos-http-api) (thanks Man, you got this thing going for me)
2. [THE sonos http api](https://github.com/jishi/node-sonos-http-api)

### 2.12. Music streaming server with (Docker) DAAPD

[DAAPD](https://github.com/jasonmc/forked-daapd) is an amazing example of how powerful, streamlined and slick open source projects have become over the years.

I’ve considered multiple options to stream audio that I won’t go in to detail here, but DAAPD has been thus far the one that is free, easy to setup, stable and with great performance. (if you have the spare cash, I would suggest going Roon - but you’ll need a server)

My docker-compose file for you:

```yaml
version: "2.1"
services:
  daapd:
    #image: ghcr.io/linuxserver/daapd
    image: keesromkes/daapd:latest
    container_name: daapd
    network_mode: host
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
    volumes:
      - ./config:/config
      - ./music:/music
      - /mnt/share/daapd_music:/mnt/share/daapd_music
      - ./backuplogs:/var/log
    restart: unless-stopped
    ports:
      - "3688:3688"
```

I know, it’s an old docker-compose version. Haven’t had the time to update yet.

![DAAPD running on my applepie](https://casey.berlin/wp-content/uploads/2021/03/2021-03-05-22.42.51.jpeg){.right}

Mind you, I’ve built my own DAAPD docker file since I wanted to get rid of a few additional features, and reduce the footprint. You can always find a good one on docker hub, as commented in the docker-compose above.

A nice little video from Darko.audio on the spoti-pi:

https://youtube.com/watch?v=-CfnXOlYiz

//todo: pihole, deconz, nodered (2x), nrgok, portainer, nginx, mqtt, raspotify, mopidy - move away audio stuff to it’s own post?

## 3. useful tools and links

1. Rcconf --> to check what services are running on your pi;

    ```shell
    sudo apt-get install rcconf
    ```

2. [Byobu](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-byobu-for-terminal-management-on-ubuntu-16-04) --> Great bit of terminal kit to be able to keep on running tasks after closing your SSH session and manage multiple windows, sessions and splits.

![Byobu showing it’s splitscreen prowess](https://casey.berlin/wp-content/uploads/2021/03/2021-03-05-23.30.41.jpeg)

3. Set your timezone, hostname etc. --> `sudo rasp-config` mostly, but I came to this because of [this article](https://howchoo.com/g/njnlzjmyyjg/how-to-set-the-timezone-on-your-raspberry-pi).

4. [Great read through on making your PI a bit tougher](https://blog.alexellis.io/hardened-raspberry-pi-nas/)

### 3.1. on using docker

#### 3.1.1. Running a docker container 'detached'

1. A luxurious name to be able to run your container without 'seeing it'; normally docker-compose would show you the output of the container until you `ctrl+c` out of it, and then stop it. Detaching means it will run without you seeing it.

    ```shell
    docker-compose up -d
    ```

2. Will make sure that it now runs on it's own. you'll see that in your terminal, and can check if it runs by:

    ```shell
    docker stats
    ```

It neatly shows you the running containers!

3. Now if you’d like to see the logs, use the following command:

```shell
docker logs homebridge
```

**Note**: `ctrl+c` out of the logs will **not** stop the container!

#### 3.1.2. Shutting down your container

```shell
docker-compose down
```

(easy huh...)

#### 3.1.3. Log in to the docker container

1. Use `docker ps` or `docker stats` to get the name of the existing container.
2. Use the command `docker exec -it <container name> /bin/bash` to get a bash shell in the container. (assuming the container has bash in it’s image). `exit` will get you out.
3. Or directly use `docker exec -it <container name> <command>` to execute the command you specify inside the container. This can be useful for updating NPM packages for instance on a node.js based app container.

#### 3.1.4. Update all your docker images

```shell
docker images |grep -v REPOSITORY|awk '{print $1}'|xargs -L1 docker pull
```

I’m not using this one often since it takes time, and it will update the images for all containers, not always knowing what changed.

#### 3.1.5. Connect a finder folder to SSH / SFTP (connect from your mac to the Pi)

Your pi will show up as ‘bonjour enabled SFTP’ drive already in apps like transmit, but if you want to enable access through SSH regularly, here’s how to do it.

1. [Here's a thread](https://apple.stackexchange.com/questions/5209/how-can-i-mount-sftp-ssh-in-finder-on-os-x-snow-leopard)

2. Installing two homebrew packages (on Mac)

    ```shell
    brew cask install osxfuse
    brew install sshfs
    ```

3. Adding a volume that is connected through SSH:

    ```shell
    sshfs pi@applepie.local:/home/pi/ /Volumes/Applepie -ovolname=HOME
    ```

4. Removing it (forceful):

    ```shell
    sudo diskutil unmount force /Volumes/Applepie
    ```
