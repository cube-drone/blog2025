+++
title = "Roblox Limits How Many Experiences You Can Block"
date = 2026-02-28T12:00:00-07:00
draft = false
categories = ["software"]
tags = ["rate limiting", "bloom filter"]
description = "it actually makes sense to limit things sometiems"
images = ["limit.webp"]
+++

Folks on reddit are mad, they posted this to `/r/mildlyinfuriating` with 18,000+ upboats.

> ## Roblox sets a limit to how many experiences you can block.
>
> https://www.reddit.com/r/mildlyinfuriating/comments/1r6dmmy/roblox_sets_a_limit_to_how_many_experiences_you/
> ![](./limit.webp)
>
> What kind of absolute bullshit is this? I used up all the blocked experiences on the however many 99 nights experiences I had to block. 

<!--more-->

I figured there'd be at least one person in the comments explaining, you know, _why_ this might be the case, but no! 

So I did a big comment:

-----

@cube-drone: 

Look, I get the Roblox hate, here, but :

So, uh, practically, I work for a multiplayer social thing, on the server team, and while we allow unlimited blocks - in order to make that work I had to spend a good month _carefully_ re-engineering the way we store that data, and then building a careful fence around features that might use that data. Unbounded data is _hard_ to work with compared to bounded data!

Unlimited _anything_ is likely to cause a server failure somewhere, at some point. Any time you have a Kind Of Thing that can be unbounded, you need to think "okay, what is the amount of these that will cause the service to fail?" Anything that's "unbounded" is an error condition waiting to happen, because invariably someone (sometimes someone actively malicious) will eventually find the _amount_ of that thing it takes to ruin your day.

Most users use, like, 1-100 blocks, right? That seems like a reasonable amount of blocks for a user to have. But there's always a weird outlier, and some of our most block-happy users had hundreds of thousands of blocks. 

In the case of blocked content, imagine you do a search against 10,000,000 experiences and you have 200,000 blocked experiences: if the search wanted to guarantee that you'd be able to _see results_ it might accidentally do something stupid like running the dot product of all of those experiences and blocks, first: computers are fast, but that's 2 trillion comparisons that need to be made to service that query - and unlike a GPU, which _can_ perform a trillion operations per second (that's what a teraflop is!), a regular CPU thread of the kind that's running on your database server might devote a minute or more to that query and **NOTHING ELSE**, which is enough to _wake up your OPS team_. The query would time-out and fail, and, irritated, you'd click "refresh", starting it again a couple of times, each time a database server in the background going "HNGGGHGHNGHGNGH". If a lot of users are doing that at the same time? Suddenly you're getting paged by the CEO at 3AM on Christmas Eve.

Building "unbounded" things is, of course, possible (we, for example, have unbounded interpersonal blocks) but it involves changing up how you build major features. If a user had some stupendously high amount of blocks - 1 million users' blocked, for example - it wouldn't be great if we were storing each block as a database row, but instead we'll send you a compressed block of that information when you boot up the client, which is still like 10MB of data, but it won't kill our servers or your client. Any time a feature wants to use that data, I have to intervene and go "okay, keep in mind that whatever query you want to write is _sometimes_ going to have to unzip, parse, and load into RAM 10MB of block data before it starts doing anything". This adds a lot of effort to any feature that _touches_ blocks.

Practically, when designing any feature that has an _amount of things_ in it, my first impulse as an older, crankier, sadder developer, is to go "estimate how many of these things the average user is expected to use, now 10x that is the maximum amount we will allow", because having access to and _enforcing_ that number is crucial for how we design the system housing that data. "It's just bits in a computer somewhere" - yeah, but somewhere, it's _someone's_ responsibility to make sure that a query that breaks the service can't happen.

------

Block data is particularly pernicious because it's not like "a set of things you can page through" - when you need block data, you often need _all of the data at the same time_.

So, like, a prospective design for user-to-user block data - and this is wildly overengineering for a lot of services - would be to have a data source sharded by user, containing blocks hashed by _other user_, with minimal associated data to keep the record size from getting out of control (time of block), and then _also_ store, per-user, maybe in S3, a compressed [cuckoo filter](https://en.wikipedia.org/wiki/Cuckoo_filter) that can be downloaded by the user, unpacked, and used to test if users are "probably" blocked. (If you're willing to accept a one-in-a-million chance of a false-positive block, you can treat this source as authoritative: probably fine in a Roblox) Then, when a new user is blocked, the block is applied to the in-client cuckoo filter and sent to the server where it's added to the sharded block-storage, but then also the server will set up a background processs to download the cuckoo filter, uncompress it, make any queued changes - or, periodically, completely recalculate it from scratch. Even then, this theoretically still bounds the size of the block-list because it has to fit into the server memory of the background process, but we can assume that _you can only block users who exist_ so the actual bound is "the number of users there are", and this design should work fine well past the point that every person on earth has 10 accounts, so you're probably good. 

Most blocks can then be calculated instantly on the client side using the filter, with the ability to fall back on the server's authoritative record.

All of that is a _lot_ of work compared to "the user can have 2000 blocks", which works exactly the way you expect it to, and you could probably just slam into a regular database table and forget about for a decade. When you're pitching how long it's going to take you to build the feature, and _explain_ all of this, it's up to the business to decide whether it's worth it to spend all of the extra time and complexity building out the very complicated version of the feature - especially because every time anyone interacts with that feature in the future they're going to have to go pull out the senior dev's pages-long document describing how it fits together and now _they're_ going to have to learn how (and why) a cuckoo filter works.