* [Getting started with a new installation][1]
* [Installing the standard software][2]
* [Homebrew / cask][3]
	* [First step, install brew][4]
	* [Second step, installing useful brew packages.][5]
	* [Third step, installing useful homebrew packages][6]
	* [Fourth step, installing Mac App Store packages through the terminal][7]
* [For everyone keeping up to the end][8]
* [Things that are manual][9]

There’s one thing I love at a new job, and one thing I don’t like at all - getting a new machine.

Or, worse, setting up the machine again once in a while. Especially if Apple decides to throw in a new update.

Or I screwed up - again, in the terminal or otherwise.

To make that process simple, over the years I’ve developed a flow to get to where I want to be, setting up my machine and installing software.

_Please be aware that a lot of this contains the terminal. It can be a daunting environment at first, but once you get the hang of it, a lot faster than your regular clicky clicky mouse setup!_

## Getting Started with a New Installation
I assume we start from square one, meaning an empty device, nothing installed. For that I create a boot drive (A USB Drive works fine) using Apple’s `createinstallmedia` command you can find here; [How to create a bootable installer for macOS - Apple Support][10]

That allows me to:
1. Delete the contents of the original harddrive (make a Time Machine backup up front!)
2. Install the OS new without relying on an internet connection through recovery.

## Installing the Standard Software
Now you’ve booted into your fresh clean OS, please make sure that you **always update the OS first** before continuing with the rest.

![aerial view photography of mountain near body of water|Thomas Ciszewski|https://unsplash.com/photos/erApmfRX7eo][image-1]{.unsplash}

We’re here, ready for the terminal?
Use spotlight (CMD+space) to find the ```terminal``` application and open it.

//todo: needs a terminal screenshot

A nice party trick to start. You know the ‘software upgrade’ feature to update your OS? you can do that from the terminal. Type along;

```shell
sudo softwareupdate -ia
```

To explain this bit;
1. The `sudo` part asks your computer to use the `softwareupdate` command with `superuser do` rights, meaning it allows you to install Mac OS updates as an administrator.
2. The `-ia` part is as it’s called a ‘parameter’ or ‘flag’, in this case asking for the installation (the `i` part) of all (the `a` part) the available updates.

In case you’re interested, type `softwareupdate —help` to look at all the other available flags.

## Homebrew / Cask / Mas
Now things get _really_ interesting. We’re going to dive into [The Missing Package Manager for macOS (or Linux) — Homebrew][11]. a.k.a. BREW.

In short, use (home)brew to install applications directly onto your mac without any process of Google, click to download, getting the wrong link, finding an outdated version or forgetting what the program was again.

I’m keeping a list of all my application and utilities that brew supports close to me, so I can always get going.

### First Step, Install Brew

On [brew.sh][12] you can find a quick installer on the top;
```shell
/bin/bash -c “$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)”
```

I’m being honest in not completely knowing what this command does, but it installs a lot of stuff including the brew package manager. From that moment on, the command `brew` will work in your terminal.

### Second Step, Installing Useful Brew Packages.

Of all packages available, I currently use five:

```shell
#cask for managing bigger applications
brew install cask &&\
#cask updates for keeping casks updated
brew tap buo/cask-upgrade &&\
#cask fonts for a big repository of fonts used later
brew tap homebrew/cask-fonts &&\
#dockutil for cleaning up my doc from a fresh install
brew install dockutil &&\
#mas for installing Mac App store packages instantly
brew install mas
```

Some additional packages related to my setup:
```shell
#gpg for installing phive
brew install gpg
#and phive itself, The PHAR Installation and Verification Environment

```

(You can copy the entire scripts above if you’d like, they will work, normally. Let me know if they don’t!)

The difference between brew / homebrew is easy, ‘brew’ is only used for open source, free to use software, where homebrew also allows closed source applications to be installed, without the whole hassle of finding the software through other ways. Just don’t try to install adobe stuff.

### Third Step, Installing Useful Homebrew Packages
This list has become extensive and fluid over the years, but in my installation consists of;

```
alfred
firefox
font-cascadia
font-cascadia-mono
font-cascadia-mono-pl
font-cascadia-pl
font-hack-nerd-font
font-ibm-plex
google-chrome
iina
iterm2
onyx
visual-studio-code
```

To find your software, type:

```shell
brew search firefox
```

for firefox for instance.

It will show you where to find firefox, and allows you to install it by typing:

```shell
brew cask install firefox
```

mind you, this includes the `cask` command, since it is an application, not an open source utility. 

**Cool trick**
You can add an entire list of to install things one after the other, for instance:

```shell
brew cask install firefox google-chrome iina
```

Will install firefox _and_ chrome _and_ iina (best mac media player!)

**Exceptions**
The only thing that _doesn’t_ work are the adobe tools, or ‘software services’ like setapp. It will not install local dmg files either. (tips are welcome!)

### Fourth Step, Installing Mac App Store Packages through the Terminal

I’m not convinced if this is any quicker the first time you do it, because you need to find each unique ID belonging to each app, but boy is it quicker the _second_ time you do it!

Type along!

```shell
mas search things
```

This will search for ‘things’, my favourite GTD app. It will return you a listing of apps that contain the word things, in my case for instance:

```shell
   904280696  Things 3                       (3.12)
   966085870  TickTick: Things & Tasks To Do (3.4.52)
   892274130  All Things Money Lite          (1.4.0)
  1107759193  Doo - Get Things Done          (2.3.3)
   782925777  All Things Money               (1.4.0)
  1020146983  Explain 3D How things work     (2.4)
   577932305  Todo Things                    (1.3)
   435972315  The Next Big Thing - Lite      (1.0)
   430203668  The Next Big Thing             (1.1)
```

All I need to do is take the number, and use it to install the application again:

```bash
mas install 904280696
```

**and back to the cool trick**

```bash
mas install 904280696 1020146983 430203668
```

Will again, install all these apps. Currently I’m using:

```bash
1333542190 1Password 7 (7.4.4)
412347921 OmmWriter (1.61)
975937182 Fantastical (3.0.7)
1295203466 Microsoft Remote Desktop (10.3.8)
1055511498 Day One (4.9)
1284863847 Unsplash Wallpapers (1.3.1)
904280696 Things (3.11.2)
931657367 Calcbot (1.0.7)
990588172 Gestimer (1.2.5)
```

I will be ready by running:

```bash
mas install 1333542190 412347921 975937182 1295203466 1055511498 1284863847 904280696 931657367 990588172 
```

And I’m good to go!

## For Everyone Keeping up to the End
A few additional tricks that I like;

1. Using dockutil to clean out your dock with inactive applications:
`dockutil — all` (you can also use it to add your standard dock items)
2. Regularly update your brew packages using `homebrew update`
3. Using Onyx to get rid of a lot of standard annoyances of my mac, like animations of the dock, transparency, and others.
4. I use [Setapp][13] to install a bunch of other tools, that I pay a monthly fee for. The installation of setapp can’t be automated as far as I know - comments are open!

## Things that Are Manual
Do you have an idea on how to make these things automated? It would be great to do so with ‘freeware’ / open source solutions like Brew! Let me know with a quick response!

1. Copy + pasting all preferences (language settings, color settings, dock show/hide, app settings like the finder, bartender or fantastical that cost lots of time - for example)\\

2. As mentioned before, doing an automated installation of setapp apps

3. How to handle installing DMG files that reside on a network drive (I haven’t touched ansible yet, that might resolve this?)

[1]:	#getting-started-with-a-new-installation
[2]:	#installing-the-standard-software
[3]:	#homebrew-/-cask
[4]:	#first-step,-install-brew
[5]:	#second-step,-installing-useful-brew-packages
[6]:	#third-step,-installing-useful-homebrew-packages
[7]:	#fourth-step,-installing-mac-app-store-packages-through-the-terminal
[8]:	#for-everyone-keeping-up-to-the-end
[9]:	#things-that-are-manual
[10]:	https://support.apple.com/en-us/HT201372
[11]:	https://brew.sh/
[12]:	brew.sh
[13]:	https://go.setapp.com/invite/2e2fbd77-9341-4553-b888-3c081ab78728

[image-1]:	https://casey.berlin/wp-content/uploads/2021/03/aerial-view-photography-of-mountain-near-body-of-water.jpeg