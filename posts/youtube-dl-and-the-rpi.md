
It took me a while to figure out how to work with the Raspberry Pi. Especially when doing (and trying to remember) harder terminal commands. Let’s take a look at getting YouTube video’s downloaded for later use, using _only_ a Raspberry Pi to do so. (and watching on a device of your choosing, iOS/mac in my case)

## what you need to get started

1. Raspberry Pi, I’ve used the 4 for this since it’s a decent machine, 2 GB of memory cuts it for me. ([full setup doc here][1])
2. External storage, for testing a SD card will do — for more than that, don’t.
3. A file-sharing setup for devices to access the files (e.g., SMB or AFP)
4. A decent internet connection (files can get pretty big)
5. A device to play your downloaded videos on
6. A pinch of patience

## Getting your installation going

For now, [youtube-dl][2] can be downloaded freely, the easiest way using `wget` or `curl` to get you going:

```shell
sudo curl -L https://yt-dl.org/downloads/latest/youtube-dl -o /usr/local/bin/youtube-dl
```

This downloads and adds the youtube-dl binary to your `usr/local/bin` folder on your device.

You’ll need to allow execution and reading rights:

```shell
sudo chmod a+rx /usr/local/bin/youtube-dl
```

And you’re ready to go! Try it out to get started:

```shell
 youtube-dl -f 'bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best <youtube videoID>'
 # this gets the best audio and video format
```

## My setup additions

I have a few use cases to download YouTube playlists:

1. Getting videos for working out to, learning more about my body
2. Music video’s I love — YouTube is a treasure chest for great 90s music or live-sets
3. ‘up to date’ content that I’d like to watch straight away

I’m using cronjobs for doing this (using the crontabs ui, that I’ll write about later) to achieve:
//todo: documentation on crontabs

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

I save the bash script for each of the playlists I want to save, and run it as a cronjob regularly.

It looks like a lot for a single command, but in short:
* `add-metatadata` adds the metadata to the file afterwards, which helps with additional information when previewing
* `write-thumbnail` writes a thumbnail of the video. It shows you the preview thumbnail in supported software.
* `download-archive` will keep an archive of downloaded files in a text file, which prevents downloading over, and over again.
* Output is stored in the download folder, with the filename including an `epoch` timestamp to be able to sort files easier, and keeping track of recent downloads

## playback software

I’ve had the most success with [Infuse][3] on AppleTV and iOS, supporting playlists — instant playback without video format problems (VC9 or MP4) and syncing video status.

![Music downloads previewing using infuse][image-1]

It’s great with metadata and thumbnails, which makes it look better.

Bonus tip for the Mac users out there, [IINA][4]. Unknown, but the best player out there!

[1]:	https://casey.berlin/pages/raspberry-pi-101/
[2]:	https://youtube-dl.org
[3]:	https://firecore.com/infuse
[4]:	https://iina.io

[image-1]:	https://casey.berlin/wp-content/uploads/2021/03/Music-downloads-previewing-using-infuse.jpeg