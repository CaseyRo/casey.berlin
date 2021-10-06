---
Title: Downloading the best quality of your youtube videos to watch offline using your raspberry pi
ID: urn:uuid:32796c93-aebd-4d72-b2ba-d73ad7ee1bf9
Category: personal interests,raspberry pi
Tags: WIP
Draft: false
Excerpt: It’s a legal grey area, but I want to share how I’m getting my videos offline to watch later.
---

It took me a while to figure out how to work with the raspberry pi. Especially when doing harder terminal commands. Let’s take a look at getting youtube video’s downloaded for later use, using _only_ a raspberry pi to do so. (and watching on a device of your choosing, iOS/mac in my case)

## what you need to get started

1. RPi, I’ve used the RPi4 for this since it’s a decent machine, 2 GB of memory cuts it for me. ([full setup doc here](https://casey.berlin/pages/raspberry-pi-101/))
2. External storage, I can’t recommend your SD card for this.
3. A filesharing setup for devices to access the files (e.g. SMB or AFP)
4. A decent internet connection
5. A device to play your downloaded videos on
6. A pinch of patience

## Getting your installation going

For now, [youtube-dl](https://youtube-dl.org) can be downloaded freely, the easiest way using `wget` or `curl` to get you going:

```shell
sudo curl -L https://yt-dl.org/downloads/latest/youtube-dl -o /usr/local/bin/youtube-dl
```

This downloads the youtube-dl binary to your `usr/local/bin` folder on your RPi.

You’ll need to allow execution and reading rights for your admins:

```shell
sudo chmod a+rx /usr/local/bin/youtube-dl
```

And you’re ready to go! Try it out to get started:

```shell
 youtube-dl -f 'bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best <youtube videoID>'
 # gets the best audio and video format
```

## My setup additions

I have a few reasons to download a few youtube playlists:

1. Getting videos for working out to, learning more about my body
2. Music video’s I love - youtube is a treasure chest for great 90’s music or livesets
3. ‘up to date’ content that I’d like to watch straight away

I’m using cronjobs for doing this (using the crontabs ui, that I’ll write about later) to achieve:
//todo: documentation on cron

* checking a playlist every 5 minutes for updates
* doing an hourly download of workout videos
* downloading music once every day

they’re all loosely based on the following bash script:

```shell
dlpath="/mnt/share/youtube/" #sets the path to download files to
dlfolder="watchnow/" #set the folder
playlist="playlistID" #set what playlistID to use to download
/usr/local/bin/youtube-dl --restrict-filenames --add-metadata --yes-playlist--write-thumbnail \
--download-archive "$dlpath$dlfolder/listing.txt" \
-o\
"$dlpath$dlfolder(%(epoch)s) %(title)s.%(ext)s" \
https://www.youtube.com/playlist?list=$playlist
```

it does a lot, but in short:
* `add-metatadata` adds the metadata to the file afterwards, which helps with more information when previewing
* `write-thumbnail` writes a thumbnail of the video. It shows you the preview thumbnail in supported software
* `download-archive` will keep an archive of downloaded files in a text file, which prevents downloading over and over again.
* Output is saved in the download folder, with the filename including an `epoch` timestamp to be able to sort files easier, and keeping track of recent downloads

## playback software

I’ve had the most success with [Infuse](https://firecore.com/infuse) on AppleTV and iOS, supporting playlists - instant playback without video format problems (VC9 or mp4) and syncing video status.

![Music downloads previewing using infuse](https://casey.berlin/wp-content/uploads/2021/03/Music-downloads-previewing-using-infuse.jpeg)

It’s also great with metadata and thumbnails, which makes it great to watch with later.

Bonus tip for the mac users out there, [IINA](https://iina.io). Bit unknown but the best player out there!