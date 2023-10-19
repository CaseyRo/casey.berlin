This all began with my passion for music and my interest in technology. It's just another iteration of what I had previously built with [Lighting Up Love - How I Created a Coldplay-Inspired LED Wristband Show for My Wedding](https://casey.berlin/lighting-up-love-how-i-created-a-coldplay-inspired-led-wristband-show-for-my-wedding/). I've utilized chatGPT extensively to shape this idea into a 'product', and I thoroughly enjoyed the process. So, what better way to share this experience than through a blog post, especially one that's also influenced by chatGPT? Why not in an interview style?

**Interviewer:** Today, we're joined by the Casey of the innovative home automation system, "Tune Orbit." First off, can you give our readers a brief introduction to what "Tune Orbit" is?

**Casey:** Absolutely! "Tune Orbit" is a home automation system I developed that 'follows' us around the house and plays music in whichever room we're in. It's like having your personal DJ that knows exactly where you are.

**Interviewer:** That sounds fantastic! What inspired you to create such a system?

**Casey:** I've always been a big futurist and enjoy experimenting with new technology. The idea came from wanting to enjoy my music seamlessly as I moved from room to room without any interruptions.

Not only that, it also needs to be sustainable. My wife already finds it annoying that I leave one light on, and for myself, I don't like to hear music coming from a room I'm not in.

And last but not least, music not in sync coming from different rooms might be even more annoying. So, "Tune Orbit" was born!

**Interviewer:** Can you dive a bit deeper into the tech behind this? How does "Tune Orbit" detect your location in the house?

**Casey:** I use general-purpose mmwave location sensors (like these from Tuya or [aqara](https://amzn.to/3LXKd2F)) to detect the persons in the rooms. These sensors offer constant, precise indoor tracking, allowing the system to know exactly where I am.

I've started using these in my office (when I'm not there it turns off the lights, when I come back to my desk after 10 minutes it turns on the power to my display and standing desk etc.) and have slowly expanded them through the house.

With those sensors I already know when a room is occupied, so why not just enable music when you're there?

**Interviewer:** And how do you manage the behaviour of the system?

**Casey:** I integrated the sensors with Node-RED and Home Assistant. This combination offers much flexibility and allows me to create the desired behaviour for "Tune Orbit."

It takes a bit of fiddling around and finding the right software bits to put together. Home assistant, node-red but also OwnTone made the right combination without becoming too complex to handle.

OwnTone's Spotify integration is also solid, the user interface is good, and their multi-room streaming is stable (as far as I've noticed, that is!)

**Interviewer:** This sounds like a complex system. Were there any challenges you faced while creating it?

**Casey:** There are a few edge cases that I needed to solve. Consider the following, for instance:

1. Let's say you're doing household chores, making you walk around from room to room. It gets a bit annoying when the audio gets disabled as soon as you leave the room, so I also included a delay that is tunable per room. 
2. We have a single room with three distinct areas (kitchen, dining table and TV part). The speakers are in the kitchen area, and I don't want to hear them when I'm watching TV. These if/then scenarios needed some finetuning.
3. Also, the volume of the speakers needed some basic tuning - so by default the speakers will turn on at volume level X (but only once)
4. Movable speakers like the Sonos roam/move; they should be excluded from turning off again if no one is present. They could be moved around, for instance. 
5. As with all edge cases, they should also be easily ignored, of course ;-) 

![tune orbit logo](https://casey.berlin/wp-content/uploads/2023/10/tune-orbit-logo.jpeg) 

**Interviewer:** The logo and visual representation of "Tune Orbit" is quite striking. Can you tell us more about it?

**Casey:** Thank you! The logo represents the essence of "Tune Orbit" – music that follows you. It's a blend of musical notes, a person, and a touch of love, showing the heart and soul I put into this project. It was also generated with DALL-E3, which recently got integrated into chatGPT. It makes visualizing ideas even easier!

**Interviewer:** Any plans to add more features or integrations to it in the future?

**Casey:** As of now, I'm quite satisfied with how it functions. But who knows? With technology evolving every day, there's always room for improvement and new features!

**Interviewer:** Thank you for sharing your journey with us. "Tune Orbit" sounds like a game-changer for music lovers!

**Casey:** It was my pleasure. I hope others are inspired to experiment and create their own unique home automation. Music is a universal language, and with "Tune Orbit," it's always by your side.