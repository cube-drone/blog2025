+++
title = 'wormhole'
date = 2024-12-26T12:00:00-07:00
draft = false
categories = ["technology", "software", "recommendations"]
tags = []
+++

I needed to send a big file to another technically competent adult

there are many ways to do this that I have access to, but I just tried a way I had never tried before, magic wormhole

https://magic-wormhole.readthedocs.io/en/latest/

It goes like this: I already have [choco](https://chocolatey.org/) installed on my computer (very like `brew` for Mac Os)

so I installed magic wormhole with a `choco install magic-wormhole`

then, I found the file I wanted to send and set up a `wormhole send file.zip`

    Wormhole code is: 11-virginia-shamrock
    On the other computer, please run:

    wormhole receive 11-virginia-shamrock

and the other computer installed wormhole, typed that in, and the file was transferred

_magic_

neat!