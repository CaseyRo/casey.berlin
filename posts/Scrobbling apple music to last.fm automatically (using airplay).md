I've been looking for a way to scrobble last.fm from my airplay plays, and I have found (a bit of a workaround) to be able to do this successfully.

**Ingredientslist**
1. Raspberry PI (3 or 4)
2. DAC (Digi pro 2+ in my case, but any should work, even your regular PI line-out)
3. A receiver to output your audio to

I've streamed my airplay to a receiver up til now, it works great - but of course, this lacked the scrobbling part.

## Here Comes Volumio!

As far as my testing goes, volumio has passed all my test regarding functionality, simplicity and user interface design, without losing flexibility.

![volumio playing airplay](https://casey.berlin/wp-content/uploads/2023/03/volumio-playing-airplay.jpeg) 

It does Spotify (connect) well, works for Bluetooth, line-in feeds, and functions well as a roon bridge. Using FusionDSP also allows for EQ adjustments based on room measurements, and the interface has cleaned up nicely over time.

It also allows for last.fm scrobbling, which I ignored for Spotify but use for all my other sources (mostly radio). And well, that also works for Airplay!

![last.fm scrobbling with airplay](https://casey.berlin/wp-content/uploads/2023/03/last.fm-scrobbling-with-airplay.jpeg) 