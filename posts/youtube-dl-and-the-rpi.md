It took me a while to figure out how to work with the Raspberry Pi. Especially when doing (and trying to remember) harder terminal commands. Let’s take a look at getting YouTube video’s downloaded for later use, using _only_ a Raspberry Pi. (and watching on a device of your choosing, iOS/mac in my case)

## What You Need to Get Started

1. Raspberry Pi. I’ve used the 4 for this since it’s a decent machine. 2 GB of memory cuts it for me. ([full setup doc here][1])
2. External storage. For testing an SD card will do — for more than that, don’t.
3. A file-sharing setup for devices to access the files (e.g., SMB or AFP)
4. A decent internet connection (files can get pretty big)
5. A device to play your downloaded videos on
6. A pinch of patience

## Getting Your Installation Going

For now, [yt-dlp](https://github.com/yt-dlp/yt-dlp) (thanks for the update commenter!) can be downloaded freely, the easiest way using `wget` or `curl` to get you going:

```shell
sudo curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp
sudo chmod a+rx /usr/local/bin/yt-dlp  # Make executable
```

Other installation methods can be found [on the github page of YT-DLP](https://github.com/yt-dlp/yt-dlp/wiki/Installation).

This downloads and adds the yt-dlp binary to your `usr/local/bin` folder on your device.

And you’re ready to go! Try it out to get started:

```shell
 yt-dlp -f 'bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best <youtube videoID>'
 # this gets the best audio and video format
```

## My Setup Additions

I have a few use cases for downloading YouTube playlists:

1. Getting videos for working out to – learning more about my body
2. Music videos I love — YouTube is a treasure chest for great 90s music or live-sets
3. Downloading my watch later queue

I’m using cronjobs for doing this (using [the crontabs ui](https://github.com/alseambusher/crontab-ui)) to achieve:

* checking watchlater every 5 minutes for updates
* doing an hourly download of workout videos
* downloading music once every day

they’re all loosely based on the following bash script:

```shell
dlpath="/mnt/share/youtube/" #sets the path to download files to
dlfolder="watchnow/" #set the folder
playlist="playlistID" #set what playlistID to use to download

/usr/local/bin/yt-dlp --restrict-filenames --add-metadata --yes-playlist--write-thumbnail \
--download-archive "$dlpath$dlfolder/listing.txt" \
-o\
"$dlpath$dlfolder(%(epoch)s) %(title)s.%(ext)s" \
https://www.youtube.com/playlist?list=$playlist
```

I save the bash script for each of the playlists I want to keep, and run it as a cronjob regularly.

It looks like a lot for a single command, but in short:
* `add-metadata` adds the metadata to the file afterwards, which helps with additional information when previewing
* `write-thumbnail` writes a thumbnail of the video. It shows you the preview thumbnail in supported software.
* `download-archive` will keep an archive of downloaded files in a text file, which prevents downloading over and over again.
* Output is stored in the download folder, with the filename including an `epoch` timestamp to be able to sort files more manageable, and keep track of recent downloads.

## Script Issues
- What I've seen happen is that it takes longer for a video to download then the 5 minute gap between the download sessions. I haven't found a solution there yet, I resorted to increasing the 'how often download' timer to 15 minutes instead. When I urgently need something I just watch it directly, or run the script manually.

## Playback Software

I’ve had the most success with [Infuse][3] on AppleTV and iOS, supporting playlists — instant playback without video format problems (VC9 or MP4) and syncing video status.

![Music downloads previewing using infuse][image-1]

It’s great with metadata and thumbnails, which makes it look better.

Bonus tip for the Mac users out there, [IINA][4]. Unknown, but the best player out there!

[1]:	https://casey.berlin/pages/raspberry-pi-101/
[2]:	https://youtube-dl.org
[3]:	https://firecore.com/infuse
[4]:	https://iina.io

[image-1]:	https://casey.berlin/wp-content/uploads/2021/03/Music-downloads-previewing-using-infuse.jpeg