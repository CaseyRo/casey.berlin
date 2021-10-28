Disaster recovery.

I’m using one of the four RPi at home to power two old speakers I’ve bought on eBay, they’re fantastic to listen to, and I’m surprised again and again at their quality and clarity (Phonar M3’s in case you’re interested).

I was listening to a bit of music when my brain decided I needed to back up my RPi, just in case. And when some files didn’t want to back up, I decided to call the following command:

```bash
sudo chmod -R 777 /etc
```

And before I knew it, I couldn’t log in anymore through SSH. With that, the inspiration for this post arrived.

I have two scenario’s when using RPi to recover now.

# Scenario 1: Backup and restore

I’m using [restic][1] for this purpose. It’s super quick, efficient, works out of the box when your Pi is [freshly installed][2] and it’s easy to restore.

- Install restic through `apt-get install restic` (might need to sudo)
- Connect to your network server (I’m using NFS - amazon, dropbox and others are supported)
- Restore your backup with restic; `restic restore` ([more details here][3])

# Scenario 2: Flat fresh install

Since the first scenario didn’t work for changing the rights on my `/etc/` folder - pasting what I needed to do as a reminder for myself:

- Setup RPi according to [my own manual][4]
- Setup `/etc/fstab` to mount the right folders

```bash
# <file system>     <dir>       <type>   <options>   <dump>	<pass>
10.10.0.10:/backups /var/backups  nfs      defaults    0       0
`
```

- Setup hifiberry Amp2 ([manual][5]) — this thing truly is a box of magic!
- Run the following script:

```bash
sudo apt install -y zsh byobu mc restic docker-ce docker-ce-cli containerd.io python3-pip neofetch figlet
```

This script installs a bunch of software using apt (ubuntu's package manager) with:

- ZSH (I prefer [.oh-my-zsh][6] on top)
  - Byobu
  - MC — Midnight commander
  - Restic
  - Python 3's package manager (pip3)
  - Docker
  - Neofetch / figlet (fun stuff for the terminal)

_I would like to point out that Docker is messy using apt, [read more here.][7]_

- Add the RPi to swarm

  - Run `docker swarm join-token worker` on your host node
  - Run the `join` command you get in return on your worker node
  - Run `docker node ls` to see it

- Install docker compose `$ sudo pip3 install docker-compose` for scripts you run on the machine itself. (sonoshttpapi for instance isn’t a great candidate for swarm mode)

- I really like bpytop `$ sudo pip3 install bpytop` to look at my reviews, it's also pretty sexy on a big screen!

```bash
curl -fsSL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install -y nodejs
```

_Installs NodeJS / NPM_

```bash
npm i -g crontab-ui node-red
```

_Installs crontab-ui and node-red using npm_

## Crontab-ui

Crontab-ui is a web-based GUI for scheduling cron jobs, it’s easy to use, and lets you manage cron jobs without having to deal with `VI` or `nano`.

- I’ve added a small bash script to crontab-ui itself to start itself (yes, very meta)

## Restic

I’ve mentioned Restic in scenario1 before this, it’s nice and quick.

- I run a batch script called `restic-e.sh` from an NFS share (will insert the script later on)
- Add backup job to crontab, to run regularly (e.g., hourly, depending on the importance)

## Node-red

The main pain I had when ‘killing’ the machine was nodered, the flows take a lot of my time to create, not backing them up a failure I’ve made a few times now.

- Run `node-red` with `--settings settings.json and --userDir DIR`

## Roon(bridge)

Roonbridge is how I get music played on my RPi. The software is outstanding, managing music ‘fun’, and it supports countless options for DSP to optimise my speaker setup.

- Install [Roonbridge][8]

Shairport-sync is a reverse engineered project that allows me to AirPlay(2) content on my RPi. It’s quick, responsive and the delay minimal.

- Build [Shairport sync 2 beta][9]

## Finally:

- Copy the following bits from your \~ (home) directory:
  - /.byobu
  - /.config
  - /.gitconfig
  - /.oh-my-zsh
  - /.profile
  - .zprofile
  - .zsh_history
  - .zshrc

The last step is personal preference, I have a specific byobu / zsh configuration I enjoy working with. YMMV.

Comments? Questions? You are welcome to leave your remarks below!

[1]: https://restic.net
[2]: https://casey.berlin/raspberry-pi-101/
[3]: https://restic.readthedocs.io/en/stable/050_restore.html#restoring-from-a-snapshot
[4]: https://casey.berlin/raspberry-pi-101/
[5]: https://www.hifiberry.com/docs/software/configuring-linux-3-18-x/
[6]: https://github.com/ohmyzsh/ohmyzsh
[7]: https://docs.docker.com/engine/install/debian/#install-using-the-repository
[8]: https://help.roonlabs.com/portal/en/kb/articles/linux-install#Roon_Bridge_armv7hf
[9]: https://github.com/mikebrady/shairport-sync/blob/development/BUILDFORAP2.md
