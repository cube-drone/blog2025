+++
title = "A Private Mastodon"
date = 2026-02-03T12:00:00-07:00
draft = false
categories = ["technology"]
tags = ["community", "mastodon"]
description = "this could be interaction fiction too, if I let it"
images = ["xoxi.png"]
+++

{{<imgwebp src="xoxi.png">}}

<!--more-->

So, aside from spending the weekend building up my [linux tolerance, somewhat](/posts/2026/the_year_of_the_linux_desktop/), I've also launched a new project.

I've launched a private Mastodon server:

https://xoxi.ca/

Now, I've actually been happily using Mastodon for _4 years_ at this point, and I don't intend to abandon my primary identity on the most popular Mastodon server: 

https://mastodon.social/@cube_drone

### So what's XOXI for, then? 

Well, it's for _weird throwaway accounts_. 

One of the things I always liked about Twitter was, you know, that moment when someone would create **A Character**, and post **In Character**, and **commit to the bit**, and you could just follow "PHP CEO" and watch as he shouted incoherently about burndown points and not understanding kubernetes.  I'd link to some examples, but I can't, really, because X/Twitter no longer allows unauthenticated access.

There's nothing stopping me from building that kind of account on one of the major silos, but honestly, creating a lot of different accounts and hopping between them willy-nilly? That's a pattern of behavior that seems a lot like abuse to any watchful operators. I think it makes sense to have a separate silo for any and all of these experiments. 

### Running Your Own Mastodon Ain't Easy

Launching a Mastodon instance is non-trivial. Mastodon is built like a modern Ruby project, which is to say, it's got dependencies on Postgres and Redis and sidekiq and elasticsearch and S3 (Wasabi) and SMTP, and setting it up involves creating 17 different individual credentials and setting up a networking cluster between authenticated services, it is a "this is my day job" amount of effort. 

I still have to write a little extra `systemd` script to schedule a `pgdump` to S3 pipeline, for backups, (maybe next weekend) which is a non-trivial amount of additional work. 

### What Kind of Weird Throwaway Accounts?

Okay, so, I launched the first one today: 

https://xoxi.ca/@fortune

> I'm sure you're tired of too-general fortunes from daily astrologers who don't take the craft seriously. That's why I'm here to provide you with professional, top quality prognostication, using astrology, tarot, phrenology, phlebotomy, tea leaves, bamboo shoots, chicken bones, palmistry, numerology, oneiromancy, scrying, cold reading, and good old-fashioned mountebanksmanship. 
> 
> Follow me for a Guaranteed Accurate™️ fortune/reading every single day.

I was thinking of writing a bot for this, but instead I just downloaded a [client, Tuba](https://itsfoss.gitlab.io/post/tuba-is-a-magnificent-new-mastodon-app-for-linux/), that allows me to schedule toots in advance: **Today's Fortune Is....** will just spit out a fortune every day at 8:00AM PST. (After scheduling a month of fortunes in advance, I'm thinking I _still_ might consider writing a bot, because the scheduling UI is a little bit fussy.)

ex: 

> Mars is in alignment with the A Priori supercluster, which we all know means that culottes are going to be in style again before long.

I wrote out fortunes to carry me to the end of March, at which point I can decide _how/if_ I want to proceed with that. One idea was just to keep writing until I had a 365 day set and then loop it at the end[^2]. 

[^2]: Writing 365 of a thing always seems extremely doable until I open a notepad and get no more than 50 things down before my brain utterly collapses.

As with most of my clever ideas, I realized about halfway through baking it that it was [just someone else's funny idea](https://www.youtube.com/watch?v=sPvyrWYutDQ). Ah, well: Marvel Rivals is one of the most successful games going right now, and it's just someone else's idea (Overwatch), which was just someone else's idea (Team Fortress 2). There's no originality under the sun. Maybe **my** fortunes will be funny?

> "That's not very impressive, for a self-fulfilling prophecy", you will think after reading this.

Other loose ideas I had scribbled down for this include:

* Official McDonald Canada :registered: Brand Account Manager, posting increasingly unhinged edits of real McDonalds advertising material
  * this is only viable until I get a cease-and-desist, which actually should take a good long time because nobody is on the fediverse but me
* A fella gradually doing a walkthrough of a video game that doesn't actually exist.
* Animal Farm, but told through the point of view of a bunch of cute Animal Crossing-ish characters.
* I just wrote "SAFETY TIPZ" here, not sure what I meant by that

### Friends Can Participate Too, If They Want

I kinda doubt I have friends who'd actually want to do this, though. 

### Why Mastodon? Mastodon sucks!

I'm honestly not sure where to put content on the internet any more. If I wanted an actual _audience_ I should be making short form funny TikToks where I share slightly misspelled life hacks while someone plays Subway Surfer, but I think what I want is to make _old internet_ content. Stuff you have to read. Bad flash games. Websites. 

I've really started to like Mastodon over the past few years. No algorithm, it's a sedate social network full of linux admins, game developers, socialists, and germans who like to post pictures of their dinner. It's a good reminder of what pre-algorithm Twitter was like: just a bunch of people posting whatever they were interested in, heedless to whether or not it was actually interesting.

I tried BlueSky, and bounced off BlueSky _basically immediately_: it's still algorithm-laden and jumping in to that ecosystem I just got buried immediately under the Democratic equivalent of endless slop memes - it feels like Truth Social with the colors swapped and less active fascism. 

As a nice bonus to Mastodon? Actual engagement. I had thousands of twitter followers, but since most of my posts weren't _optimized for engagement_, few of them ever saw my tweets, and that became _much worse_ in the new regime. Within the fediversee, the only thing that stops my thousand followers from seeing my toots is that _most of them have got bored and left forever_, but that still leaves a sizeable cadre of people who are still interested in the platform. 

With careful curation, my timeline is pretty usable too. Do you know what people post on Mastodon? Links to blog posts on their personal blogs (still online). People making their own video games (awful). Bots that repost local news articles. Funny neurodivergents. Oil paintings. And I can finish scrolling after half an hour because _that's everything I've subscribed to and there's nothing more_. 

Sometimes ghost towns are a nice place to spend your time.

> ## The decline of Snapchat and the secret joy of internet ghost towns
>
> https://www.theverge.com/2018/5/18/17366528/snapchat-decline-internet-ghost-towns
> 
> As fewer and fewer of my friends use the platform, Snapchat has become a haven from the grinding utility of the internet