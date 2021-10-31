*Long story short, I needed access to my RPi files after I wasn’t able to access them anymore. Those files were stored on an ext4 partition on my SD card, and my mac doesn’t read them.*

If that is what you’re looking for, congratulations! I hope this post will solve things for you, since I had to do this!

And if you’re like me, don’t simply `chmod` your `/etc` directory. A reminder for myself.

The solution at hand:

## Step 1: Run a virtualbox with ubuntu (live cd) on your mac

## Step 2: Mount your SD card into your virtualbox

To keep things simple for now, read this post:

https://www.geekytidbits.com/mount-sd-card-virtualbox-from-mac-osx/

I’ll update this post as soon as I have time, I want to make sure you’re finding what you need here.