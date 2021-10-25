Disaster recovery.

I’m using one of the four RPi at home to power two old speakers I’ve bought on eBay, they’re fantastic to listen to, and I’m surprised again and again at their quality and clarity (Phonar M3’s in case you’re interested).

I was listening to a bit of music when my brain decided I needed to back up my RPi, just in case. And when some files didn’t want to back up, I decided to call the following command:

````sudo chmod -R 777 /etc```

And before I knew it, I couldn’t log in anymore through SSH. With that, the inspiration for this post arrived.

I have two scenario’s when using RPi to recover now.

# Scenario 1: Backup and restore

I’m using [restic](https://restic.net) for this purpose. It’s super quick, efficient, works out of the box when your Pi is [freshly installed](https://casey.berlin/raspberry-pi-101/) and it’s easy to restore.

* Install restic through ```apt-get install restic``` (might need to sudo)
* Connect to your network server (I’m using NFS - amazon, dropbox and others are supported)
* Restore your backup with restic; ```restic restore``` ([more details here](https://restic.readthedocs.io/en/stable/050_restore.html#restoring-from-a-snapshot))

# Scenario 2: Flat fresh install

Since the first scenario didn’t work for changing the rights on my `/etc/` folder - pasting what I needed to do as a reminder for myself:

* Setup RPi according to [my own manual](https://casey.berlin/raspberry-pi-101/)
* Setup `/etc/fstab` to mount the right folders

```bash
# <file system>     <dir>       <type>   <options>   <dump>	<pass>
10.10.0.10:/backups /var/backups  nfs      defaults    0       0
````

* Setup hifiberry Amp2 ([manual][1]) — this thing truly is a box of magic!
* Run the following script:

```bash
sudo apt install -y zsh byobu mc restic docker-ce docker-ce-cli containerd.io python3-pip neofetch figlet
```

This script installs a bunch of software using apt (ubuntu's package manager) with:

* ZSH (I prefer [.oh-my-zsh][2] on top)
	* Byobu
	* MC — Midnight commander
	* Restic
	* Python 3's package manager (pip3)
	* Docker
	* Neofetch / figlet (fun stuff for the terminal)

*I would like to point out that Docker is messy using apt, [read more here.][3]*

* Add the RPi to swarm
	* Run `docker swarm join-token worker` on your host node
	* Run the `join` command you get in return on your worker node
	* Run `docker node ls` to see it

* Install docker compose `$ sudo pip3 install docker-compose` for scripts you run on the machine itself. (sonoshttpapi for instance isn’t a great candidate for swarm mode)

```bash
curl -fsSL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install -y nodejs
```

*Installs NodeJS / NPM*

```bash
npm i -g crontab-ui node-red
```

*Installs crontab-ui and node-red*
Crontab-ui is a web-based GUI for scheduling cron jobs, it’s easy to use, and lets you manage cron jobs without having to deal with `VI` or `nano`.

* [Crontab-ui][4]
	* I’ve added a small bash script to crontab-ui itself to start itself (yes, very meta)

I’ve mentioned Restic in scenario1 before this, it’s nice and quick.

* Restic
	* I run a batch script called `restic-e.sh` from an NFS share (will insert the script later on)
	* Add backup job to crontab, to run regularly (e.g. hourly, depending on the importance)

The main pain I had when ‘killing’ the machine was nodered, the flows take a lot of my time to create, not backing them up a failure I’ve made a few times now.

* Nodered ([install with NPM][5])
	* Run node-red with `--settings settings.json and --userDir DIR`

Roonbridge is how I get music played on my RPi. The software is outstanding, managing music ‘fun’, and it supports countless options for DSP to optimise my speaker setup.

* Install [Roonbridge][6]

Shairport-sync is a reverse engineered project that allows me to AirPlay(2) content on my RPi. It’s quick, responsive and the delay minimal.

* Build [Shairport sync 2 beta][7]

Finally:

* Copy the following bits from your \~ (home) directory:
	* /.byobu
	* /.config
	* /.gitconfig
	* /.oh-my-zsh
	* /.profile
	* .zprofile
	* .zsh\_history
	* .zshrc

The last step is personal preference, I have a specific byobu / zsh configuration I enjoy working with. YMMV.

Comments? Questions? You are welcome to leave your remarks below!

[1]:	https://www.hifiberry.com/docs/software/configuring-linux-3-18-x/
[2]:	https://github.com/ohmyzsh/ohmyzsh
[3]:	https://docs.docker.com/engine/install/debian/#install-using-the-repository
[4]:	https://github.com/alseambusher/crontab-ui
[5]:	https://nodered.org/docs/getting-started/local
[6]:	https://help.roonlabs.com/portal/en/kb/articles/linux-install#Roon_Bridge_armv7hf
[7]:	https://github.com/mikebrady/shairport-sync/blob/development/BUILDFORAP2.md