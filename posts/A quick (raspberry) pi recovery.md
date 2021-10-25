For whatever reason you’re looking for a quick way to restore your raspberry pi, or actually starting to work with it quickly.

I’ve ruined my setup again and again, so I was looking for a few solutions to make restoring quicker.

# Scenario 1: Backup and restore

I’m using [restic](https://restic.net) for this purpose. It’s super quick, efficient, works pretty much out of the box when your Pi is [freshly installed](https://casey.berlin/raspberry-pi-101/) and it’s easy to restore.

* Install restic through ```apt-get install restic``` (might need to sudo)
* Connect to your network server (I’m using NFS, might need to publish something about this in the future - amazon, dropbox and others are supported)
* Restore your backup with restic; ```restic restore``` ([more details here](https://restic.readthedocs.io/en/stable/050_restore.html#restoring-from-a-snapshot))

# Scenario 2: Copy from another Pi

it’s a Linux system, so copying and pasting is also a way to go!

* connect using a network, NFS is my preferred way.
* Copy the following bits from your ~ (home) directory:
	* /.byobu
	* /.config
	* /.gitconfig
	* /.oh-my-zsh
	* /.profile
	* .zprofile
	* .zsh_history
	* .zshrc

* I assume potentially you can also copy all your `NPM` and `apt` packages (or make a backup?)

//todo: check a NPM list backup solution

# Scenario 3: Flat fresh install

I recently had to do this, so I’m going to paste what I needed to do as a reminder for myself:

* Setup Pi according to my own manual
* Setup `/etc/fstab` to mount the right folders
* Setup hifiberry Amp2 ([manual](https://www.hifiberry.com/docs/software/configuring-linux-3-18-x/))

```bash
sudo apt install -y zsh byobu mc restic docker-ce docker-ce-cli containerd.io python3-pip neofetch figlet
```

Installs a bunch of software using apt (ubuntu's package manager) with

* ZSH / .oh-my-zsh
* Byobu
* MC - Midnight commander
* Restic
* Python 3's package manager (pip3)
* Docker
* Neofetch / figlet (fun stuff for the terminal)

* Docker (might be a bit messy using apt, [read more here.](https://docs.docker.com/engine/install/debian/#install-using-the-repository))

* Add pi to swarm
	* Run `docker swarm join-token worker` on your host node
	* Run the `join` command you get in return on your worker node
	* Run `docker node ls` to see it

* Install docker compose `$ sudo pip3 install docker-compose`

```bash
curl -fsSL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Installs NodeJS / NPM

```bash
npm i -g crontab-ui node-red
```

Installs crontab-ui and node-red

* [Crontab-ui](https://github.com/alseambusher/crontab-ui)
	* Add starting of crontab-ui to itself
* Restic
	* Run restic-e.sh from NFS share
	* Add to crontab
* Nodered ([install with NPM](https://nodered.org/docs/getting-started/local))
	* Set `settings.js` to run on NFS share
	* Run node-red with `--settings settings.json and --userDir DIR`
* Install [Roonbridge](https://help.roonlabs.com/portal/en/kb/articles/linux-install#Roon_Bridge_armv7hf)
* Build [Shairplay sync 2 beta](https://github.com/mikebrady/shairport-sync/blob/development/BUILDFORAP2.md)
* Copy files according to step 2 above
