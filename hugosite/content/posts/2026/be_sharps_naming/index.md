+++
title = "Be Sharps Naming"
date = 2026-03-10T13:00:00-07:00
draft = false
categories = ["software"]
tags = ["naming", "the two hardest problems in computing"]
description = "We need a name that's witty at first, but less funny every time you hear it."
images = ["be_sharps.jpg"]
+++

{{<imgwebp src="be_sharps.jpg">}}

<!--more-->

-----

I have a real soft-spot for softly meaningful names for servers, usually also pop-culture references.

**I also, strongly, discourage clever names at work**. 

We have a long-standing "no clever names" policy, called the "Be Sharps" policy, because at my previous job they allowed unchecked proliferation of _clever developer names for things_ and it became a _problem_.

After a while, you get mad at a server called `dryad` that runs all of your loggers. Sure, it's cute, but do you know what it could have been? It could have been _useful and descriptive_. 

You could have named that server `log_server_01` and saved everybody a bunch of time looking up what each _server_ does. 

Once every service and tool has a name like this, you introduce the problem that new folk have to spend weeks learning the too-clever-by-half ha-ha naming rules behind everything at your company. 

Even worse, if you have a _lot_ of services like this - well, I also discourage _wild microservice proliferation_, and a big part of both the "clever names" restriction and the "microservices" restriction is because I have actually lived this sketch, and it is not pretty:

{{<youtube y8OnoxKotPQ>}}

> So `galactus` won't be able to find our new `birthday boy` provider, which means `wingman` won't know how to talk to anybody, which means I won't be able to find true love and I'll _die alone_. 

Every time a creatively named variable comes down the pipeline, I have to tap the sign - but at home, the gloves come off.

## Letting The Gremlin Free At Home, Where it Belongs

So, "Scratch" is the little box running in my house, and "Sovereign" is the dirt-cheap gateway VPS running on CanHost pointed at it.

* `sovereign`, obviously, because it's running in Canada
* `sovereign`, because I love The Venture Bros, and the `sovereign` was a shadowy figure that ran the Guild of Calamitous Intent _but nobody knew who it really was_ and then eventually it turned out it was _David Bowie_ - so, an okay pick for a server who's purpose it is to be the front of a shadowy secret society:

![](./sovereign.jpg)

* `scratch` because I had to re-do the whole thing from _scratch_
* `scratch` because [homestuck](https://mspaintadventures.fandom.com/wiki/Doc_Scratch).
* `scratch` because [i love cats, i love every kind of cat, I just want to hug all of them but I can't](https://www.youtube.com/watch?v=sP4NMoJcFd4).

I'm connecting to that from my new linux computer, `asceticbot`, which is named that because

* Originally the windows computer was named `hedonismbot` because I wildly overprovisioned it with a threadripper and 256GB of RAM, and [I apologize for nothing](https://www.youtube.com/watch?v=Sv4Gui9hKCM).
* But 5-6 years later, the threadripper is starting to show its age, windows is starting to suck, and instead of upgrading I'm choosing the more efficient path: moving to linux and embracing the life of a cold, lonely Linux pervert. Thus, the opposite of `hedonismbot`, `asceticbot`.

Other computers of note as of recent:

* The soon-to-be-decommissioned `girlboss` is my nginx load balancer, as it replaced `gatekeep`, the first nginx load balancer - if I launched another one it would of course be `gaslight`. (ref. [Gaslight, gatekeep, girlboss](https://knowyourmeme.com/memes/gaslight-gatekeep-girlboss))
* `marquee` is just my favorite HTML tag
* Tiff's old computer was `beast` because it was _huge_, and she was the _beauty_ I built it for.
* Her current computer is `archiebee`, because she explicitly requested RGB lighting elements in its design.
* My laptop is `thermidor` because I bought it in november and it runs pretty hot.
* The NAS is `stacks` because that's the name of [the God Of Data Storage from my elaborate personal mythology](https://books.cube-drone.com/gpm-book/twelve/4-stacks.html). 
* The phone is `fourteen` because it's a OnePlus 13

this is very satisfying

But wouldn't it be _impossibly frustrating_ if you had to work with me? Think about it. 