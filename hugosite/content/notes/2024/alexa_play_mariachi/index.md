+++
title = "alexa, play mariachi radio"
date = 2024-07-27T12:00:00-07:00
draft = false
categories = ["technology"]
tags = ["home automation", "alexa", "mariachi", "radio"]
+++

For some absolutely asinine reason I have smart lights in my house.

(I can tell you why: it started when we realized that the previous inhabitants' genius electrical work would turn the furnace off if the basement lights were switched off, so we either needed to rewire it or, cheaper, get lights that would switch off independently of the light switch)

There are, unfortunately, no smart buttons that aren't battery powered, so I made the mistake of buying some on-sale Alexa speakers.

![](./tea.png)

Amazon's smart speaker platform is _juuust_ smart enough that with a little bit of effort (about 4 hours of techno-faffing), I could get it to play an icecast stream by using a soon-to-be-deprecated developer API.

Getting my icecast radio station hooked up was a pain in the ass, but even more of a pain in the ass was finding activation phrases that were:

* Unique enough that they wouldn't trigger unwanted behavior
* Pronounceable enough that Alexa could reliably parse them.

Because "marquee.click" is my personal domain for Various Shenanigans, my first crack at this was the activation phrase "marquee radio" which had something like a 5% success rate.

Most of the time when I said "Alexa, play marquee radio", Alexa would hear "Alexa, play mariachi radio", and as much as I like non-stop mariachi tunes, it's not what I asked for.

Currently the activation phrases are

* "Alexa, buttery nunchucks" ( for my main radio station, https://radio.marquee.click)
* "Alexa, cascading bumblebees" ( for https://radio.marquee.click/chill.ogg )
* "Alexa, unbelievably boring" (podcasts)

these, of course, only work within my home: it's in dev mode, I probably couldn't get my radio stations through certification, being as I do not have the legal right to rebroadcast this music (I _paid_ for the mp3s, but the radio station is for personal use)

-----

So, I have basically perfect Machine Voice: Alexa always listens to me and interprets what I have to say correctly. This infuriates my wife, who tends towards angrily shout-mumbling at the device ("LEXA! LIGHSOFF!") and then getting _more_ angry when it doesn't do shit

My wife has an unusual pan-American accent borne from being a military kid, but she's not unclear: instead I think she's just kinda mad that the device expects clear enunciation and refuses to play it's game, whereas I'm so in love with the sound of my own voice I can't help but go "Alexa, could you please turn the lights off in the living room?"

I've got the device vibe a little better than she does. That's not to say it isn't VERY OFTEN just, like, 15% dumber than I want it to be.

A good question to ask Alexa is "what do you think I just said?" because it will often provide some context for a completely nonsensical response.

------

The pile of watch-battery-powered smart buttons have gradually worked their way into the trash. They churn through those batteries at a pretty prodigious rate, fail often, and they _don't feel good to press_.

I also won't wire things directly into the house, on the assumption that smart tech is garbage and won't last longer than the house will.

I've thought of cheap tablet or raspberry pi shit, but ugh

If someone out there knows of a plug-in-to-the-wall smart button platform out there, lmk

------

Getting Google Home to play a custom icecast stream is impossible.

Getting Alexa to play a custom icecast stream took me 3 hours of debugging against a pretty gnarly Amazon interface, a lambda deployment, and TuneIn intervening to play mariachi music A NUMBER OF TIMES.

... I'm pretty sure Alexa is better but my eyes hurt