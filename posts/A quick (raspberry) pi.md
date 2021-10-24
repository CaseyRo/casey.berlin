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
	* `bash.rc` 
	* bla
	* bla
	* bla
* I assume potentially you can also copy all your `NPM` and `apt` packages (or make a backup?)

//todo: check a NPM list backup solution

# Scenario 3: Flat fresh install

I recently had to do this, so I’m going to paste what I needed to do as a reminder for myself:

* Setup Pi according to my own manual
* Setup `/etc/fstab` to mount the right folders
* Setup hifiberry Amp2 ([manual](https://www.hifiberry.com/docs/software/configuring-linux-3-18-x/))
* Install:
	* ZSH / .oh-my-zsh
	* Byobu
	* MC - Midnight commander
	* NPM
	* Docker
		* Add pi to swarm
	* [Crontab-ui](https://github.com/alseambusher/crontab-ui)
		* Add starting of crontab-ui to itself
	* Restic
		* Run restic-e.sh from NFS share
		* Add to crontab
	* Nodered ([install with NPM](https://nodered.org/docs/getting-started/local))
		* Set `settings.json` to run on NFS share
		* Run node-red with `--settings settings.json and --userDir DIR` 
	* [Roonbridge](https://help.roonlabs.com/portal/en/kb/articles/linux-install#Roon_Bridge_armv7hf)
	* [Shairplay sync 2 beta](https://github.com/mikebrady/shairport-sync/blob/development/BUILDFORAP2.md)
* Copy according to step 2 above