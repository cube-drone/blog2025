+++
title = "MongoDB is Web Scale" 
date = 2026-07-10T12:00:00-07:00
draft = false
categories = ["software"]
tags = ["database", "mongodb", "postgres"]
description = "I'm sick of people dumping on Mongo, it's legitimately good."
images = ["booing.png"]
+++

![](./why_are_you_booing.png)

Sometimes I feel like the only remaining MongoDB fan out there. 


![](./weird_nerds.png)

See, the thing that actually most programmers remember about MongoDB is this viral meme:

{{<imgwebp src="web_scale.png">}}

<!--more-->

## MongoDB Is Web Scale

{{<youtube b2F-DItXtZs>}}

This was considered _very funny_ in 2010 because at the time there was a lot of comedy centered
around being an ornery jackass. 

It's structured as an argument between an angry, technically competent hero, and a dull strawman character,
with the straw-man gamely trying to mount an absurd, cargo-cult defense of MongoDB against our increasingly frustrated
hero's concerns.

Let's start by going at the claims of this video:

### 1. (Strawman) MongoDB Doesn't Use SQL or JOINs, so it's high performance. Relational Databases Don't Scale Because they Write To Disk.

I'm not going to go into a lot of effort debunking this, because it's obviously and on the face of it untrue.

I was there in 2010: I'm not convinced there were a lot of serious engineers in 2010 who believed that relational databases couldn't scale.

### 2. (Hero) NoSQL solutions are too immature for modern developers right now.

This was barely true in 2010, it's certainly not true now. Non-relational databases are ubiquitous. 

Time-series databases, graph databases, search databases, cache, messaging databases? Young ecosystems in 2010, well established in 2026. 

The state of backend engineering was not set in stone in [1970](https://dl.acm.org/doi/10.1145/362384.362685). 

### 3. (Hero) When You Write To MongoDB, You Don't Actually Write Anything, You Stage Your Data to be Written at a Later Time

Mongo, in order to _juice the benchmarks_, is accused of deferring the part where you write the data to disk.

Which, at the time, it... did. It was just a memory-mapped database that periodically wrote to disk. Not terribly safe.
Defensible in some systems - nowadays Redis still works like this out of the box. (Redis is great, by the way).

It's better now. Well - mostly. Mongo can still be a little cavalier about data loss - let me tell you how:

So, a "write-ahead log" - instead of putting the data where it goes, you write to a location - still _on disk_- where you
line up writes that you will perform _soon_. Because it's very fast to append to a file, this is actually a safety primitive:
the faster you get something written to disk, the less likely you are to experience catastrophic data loss.

This is how nearly _every database works_.

However - there's more than one mongo server- by design, there's a failover plan, 
and Server A is constantly replicating to Server B, so that in the case of a failure, Server B can take over. 
If Server A kicks the bucket before Server B knows about a write,
and then Server B takes over, what do you do with Server A's write-ahead-log? 

Well, Server B  has already taken over and started accepting writes on its own, those Server A writes, trapped
on a dead server, might introduce data consistency problems, so they can be discarded.

The actual design decision here, and the one that's suspicious, is that Mongo can return success from
a write before the write is _written to a replica_. This is a sane design decision when you're interested in
high write throughput, and an absolutely insane decision if you're interested in writing bank software.

But, a production Mongo server today is configured with "majority write" as the default: the write isn't complete until
it's verifiably been received not just by one server but also by the majority of servers in the cluster. This slows 
down writes, but if you've written a write: that write is well and truly written.

And you know what? A lot of teams still use Mongo in the "throughput over perfect write safety" mode. 
That's a sane trade-off to consider. 

So, yeah: 2010 Mongo Bad. Modern Mongo? Good! 

### 4. (Hero) Software Developers are More Interested in Aping Whatever's Popular Than Learning How Databases Work

While broadly true, this clip didn't make people more interested in databases, it just made MongoDB _unpopular_,
and the people aping its arguments uncritically are exactly as frustrating as the strawman in the video 
mindlessly echoing "web scale".

### 5. (Strawman) Redis will kick memcached's ass.

![](./redis.png)

This... actually turned out to be true. Redis _did_ kick memcached's ass.

### 6. (Hero) MongoDB abandons transactions, consistency, and durability.

I think this is kind of a roundabout way of saying that MongoDB isn't ACID - which was true, but even at the time
it was at least consistent at the document level, which, given Mongo's document-based model of understanding reality,
was most of what you'd want.

4.0 was a big step forward for ol' Mongo: by introducing multi-document transactions it got its ACID wings. 

ACID is absolutely necessary if you're writing financial systems. I will say this: _probably don't write
financial systems in 2010 MongoDB._ This is not the right database for dual-entry accounting. 

### 7. (Strawman) MongoDB stores files of any size without complicating your stack.

MongoDB documents currently top out at about 16MB - which is absolutely huge for a JSON file, but it's not "any size". 

GridFS can store files by chunking them into documents, but... what an odd thing to bring up. It's an extremely complicated stack! 

### It's Only Tangentially About MongoDB

The whole video is more of a long-form rant about cargo-cult development with MongoDB as the designated punching bag, 
from a time where "angry guy is angry about thing in a rude way" was peak internet humor.

## The Case for Postgres

Postgres has kind of eclipsed MySQL in popularity since this was originally written, but: SQL, in general. 

Vertical scaling goes way harder than anybody gives it credit for.

You can take a Postgres server and easily give it hundreds of terabytes of NVMe-speed disk space, 
half a terabyte of RAM, and 128 processor cores or more. The largest companies in the world weigh in at millions of employees - 
If what you're doing is payroll for any company on the planet, it will run just fine on Big Server.

Just about every backend tool can be approximated _pretty well_ in Postgres.

Do you want a queue? Build it in Postgres.

Do you want logging? Build it in Postgres.

Do you want search? Build it in Postgres.

Do you want a cache? No you don't, Postgres is plenty fast enough.

Do you want distributed locking? Write the locks in Postgres.

Backend developers learn SQL early and well. 

Thinking in relational design is how backend developers are trained. 

It's a well-understood design space.

If you're building a product for an audience of less than 10,000 people, it's borderline malpractice
to bother invoking anything outside of just regular Postgres. It is a swiss-army knife of usefulness.

However - while it's _good enough_ at everything, many of the things you can just do in Postgres,
you can do better and faster in other tools. Your cache should be in Redis (or Valkey, or ElastiCache, which
are just Redis again, wearing a variety of jaunty hats). 
Your queue should be a queue, your logs should go in a logger, 
your time-series data should go in a time-series database, your search should go in a service that does search 
really well - and doing search really well is super hard. Postgres gets it mostly right but it's not Elasticsearch.

When all you have is a SQL-shaped hammer, everything looks like a nail. 

So, this creates a sort of dichotomy of developers: safe, stable, intelligent, wise, swiss-army-knife 
developers who want to use Postgres for everything because it's good enough and adding more tools adds more complexity, 
and web-scale cargo cult magpie idiots who want to use the coolest tool to solve any specific job and deliver
a product with 38 different interlocking dependencies. 

So, in camp A, "Simplicity is a Virtue" developers, in camp B, "The Right Tool for the Right Job" developers.

Of course: they're both right. These are both design goals to strive for, and every project
lives or dies on the trade-offs it makes between these ideas.

Hi, I'm a cargo cult magpie idiot (sometimes), let me make my case.

### Let's Get Distributed

I think the most common Postgres deploy is just Postgres, alone on a server, with a daily backup. 

If the operator of a single-node Postgres stack with daily backups ever gives Mongo shit about losing 
100ms of writes in a disaster, you can safely ignore them forever - I don't know where they think their 
writes are going in a disaster scenario but they are in for a fun surprise.

> ![viking funeral](viking_funeral.png)
> pictured: gradually realizing that consistent journaling will not save your data if the hard drive is on fire

Once you're running Postgres in production, though, you're going to need a hot-swap replica: a second
Postgres server that all of your writes are headed to, so that in an emergency, you can jump over to the replica
server.

Here's the thing: Mongo is designed to be operated in a cluster - it's expected that you will cluster it, 
from the get-go, when you first get started on developing with it. The whole operational philosophy of Mongo assumes
replication from day one, so the complexity of clustering is kind of front-loaded in. This is a pain in the ass
at first, but when you want to deploy a hot replica, well, you probably _already did_. 

Postgres, on the other hand, waits until you're about _here_ to start hitting you with all of the problems of
distributed systems. Is a write complete when your main server has written it or do you wait until you've heard back
from all of the secondaries? What's your failover strategy? If your backup server loses contact with your main server
but they're both still online, what stops them from just running at the same time? 

### You Probably Don't Need Scale, You Probably Do Want Resilience

Distributing your systems is about more than just _scale_, it's also about resilience. If you have 100 servers, 
one server failing is not catastrophic. That resilience is hugely important. The more servers you have the
less you care if any individual server fails. 

So, the argument that distributed databases trade away ACID guarantees for scale that you'll never need 
kind of misses that you also get resilience that you DO need. 

### Some Things Get Huge, Even For Small-N Companies

The idea that you'll never need enormous, fabulous scale really depends on the nature and granularity of data you're 
producing or gathering.

Modern systems do an enormous amount of surveillance on their users. Each individual user is producing a stream of
data _constantly_. It turns out, even a small number of users at a high sample rate can produce an absolutely fabulous
amount of data.  

It's actually much easier to hit "petabyte scale" than you think, especially if the cost of getting a little bit
bigger is just the cost of one more little server, rather than a $100,000/month upgrade to your gigantic Postgres database.

### Some Companies Get Huge

Also: every company _does_ plan to get huge. I don't know who _you_ work for, but most products have big goals.

Designing for planetary scale might be wildly premature, but: the people who hired you want to take over the world
and it's your job to convince them you can.

### Scaling Read and Write - Can /dev/null Be Sharded?

Let's talk about scaling if you aren't Just Making The Server Bigger, because you have tools that make horizontal distribution practical. 

If you want to make a server serve more reads, you can serve data out of a replica. More replicas can always handle more read traffic.

But, every write has to go to every replica: so, if one server is 90% busy with writes, and you replicate to 10 servers, now you have
10 servers that are all 90% busy with writes.

You can always resolve this problem with vertical scaling - and, as I mentioned earlier, you can vertically scale for a lot longer
than you might think reasonable - but if you have _any_ replication - and you always should have at least
_some_ replication - you have to vertically scale _every server in your cluster_ at about the same rate.   

At some point you are going to reach a size where eternal vertical scaling looks like it might start running out. 

At this point, you've got to start looking at how to actually move write load around: and if you have decade-old data in a JOIN-heavy architecture,
unraveling this knot is going to be a years-long and very difficult process.

### Maybe JOINs _ARE_ a Little Bad?

You know, that stupid little doll thing saying that JOINs are bad for performance?

He's... not exactly _wrong_.

The whole ethos of normalization is to reduce the number of places you have to write important variables - 
you can hopefully realize why this also _optimizes_ for write bottlenecks.

This is good for correctness - but, bottlenecks are hard to unknot. 

One of the things that makes it hard to re-engineer systems are points where they interlock - the more things
that JOIN against a table, the more likely it is that that table will become a bottleneck, and the
harder it becomes to change that table going forward.

## I Don't Need to Make a Case for Redis

Postgres is great, but... fast ephemeral data, straight out of memory, accessible to all of your application servers?

I'm not here to launder the reputation of Redis, it's already doing fine. 

Everyone knows Redis is great. It is the jelly to Postgres's peanut butter.

You really do need cache, distributed locks, and in-memory data structures - Redis is as much a swiss-army-knife as Postgres is.

## Let's Make a Case for MongoDB

### It Is, In Fact, Convenient

It turns out, a tree-shaped JSON document is a really useful thing to store, and pretty easy to reason about.

Look at this user model: 

```
{
   username: "phil-mccracken",
   displayName: "Phil McCracken",
   tags: [
    "admin", 
    "currently_not_on_fire"
   ],
   displayThumbnail: "https://example.org/phil_face.jif",
   createdAt: ISODate("2024-03-01T12:48:22+0000"),
   country: "CA",
   locale: "an_us",
   passwordHash: "828d7272387d827327328d7d2873827d287d273287d27",
   validCountryLoginLocations: [
     {
        country: "CA",
        last_login: ISODate("2024-03-01T12:48:20+0000"),
     },
     {
        country: "US",
        last_login: ISODate("2021-01-01T12:00:30+0000"),
     },
     {
        country: "MX",
        last_login: ISODate("2019-01-01T12:00:40+0000"),
     },
   ]
}
```

Did you struggle to reason about this at all? No? Of course not. 

### Schema Changes Can Fail Forward

If you want to enforce a schema for a table, you _can_ - it's not like JSON Schemas don't exist.

But, a database with no fixed schema allows you to plan schema updates from the application side instead,
which can also be very powerful: adding a field is just as easy as ... having the application treat that
new field like it has _always been there, but might be null_. If you want, you can backfill that data,
and then remove your null checks after.

So long as you can keep both versions of your schema valid in code, you can smoothly move from one 
valid schema to another: migrations become a lazy switchover. 

### No, You Can't Just Plop a JSONB Field in Postgres And Get Mongo For Free

I chased this claim down - that since Postgres has JSONB, it's basically got everything Mongo has, right?

That is not true. Mongo is designed to work with complex JSON documents from the ground up: every affordance
in the system is designed to make _this thing_ practical and convenient: let's say (looking at the user model, above)
you want to write a query that finds every user tagged with "hat" who also has logged in from Canada after 2023,
and give that user a new tag, "toque", without having to roundtrip the entire document through application code? 

This looks pretty smooth in MongoDB:

```
db.users.updateMany(
  {
    tags: "hat",
    validCountryLoginLocations: {
      $elemMatch: {
        country: "CA",
        last_login: { $gte: ISODate("2024-01-01T00:00:00Z") }
      }
    }
  },
  {
    $addToSet: { tags: "toque" }
  }
)
```

And borderline horrifying in Postgres:

```
UPDATE users
SET jsonball = jsonb_set(
  jsonball,
  '{tags}',
  CASE
    WHEN COALESCE(jsonball->'tags', '[]'::jsonb) @> '["toque"]'::jsonb
      THEN COALESCE(jsonball->'tags', '[]'::jsonb)
    ELSE COALESCE(jsonball->'tags', '[]'::jsonb) || '["toque"]'::jsonb
  END,
  true
)
WHERE COALESCE(jsonball->'tags', '[]'::jsonb) @> '["hat"]'::jsonb
  AND jsonb_path_exists_tz(
    jsonball,
    '$.validCountryLoginLocations[*] ? (
      @.country == "CA" &&
      @.last_login.timestamp_tz() >= "2024-01-01T00:00:00Z".timestamp_tz()
    )'
  );
```

If you wanted to accomplish this cleanly in Postgres, you'd want tags, login locations, and the core user all connected
with separate relational tables. This query is fighting upstream against Postgres's natural data model.

There's a clean path to this query in both environments: but they both start from "playing by the database's rules". 

### It's More Modern Than You Remember

Some of the things that people complain that MongoDB doesn't have are things that it does, in fact, have.

Full-text search primitives? Multi-document transactions? Joins? Yeah, that's all in there, now. 

You _can_ write a whole application the same way that you would a Postgres application, using these tools. 

But, for the same reason you shouldn't use Postgres as a nested JSON document database, you... shouldn't
use Mongo as a relational database. The relational features aren't the point in Mongo in the same way 
that the JSON features aren't the point in Postgres. If you try and lean in to relational Mongo design 
you're going to have a bad, weird time. 

I would argue, however, that - having been forced to learn a lot of it myself - designing applications
around a nested document model is _not terrible_. Just different. 

It's a full-featured database. In fact, some of these tools - being as they're not written with decades
of SQL cruft behind them - are pretty nice to use. Try writing a complex Mongo aggregation - it's a 
really powerful language for extracting report data. 

### User Bucketing Is Broadly Applicable

Do you know an absurdly common pattern? It's "bucketing by userId". It turns out, the most common relationship 
in most systems is "who owns this thing?". It's kind of how systems like S3 work: under the hood: 
large distributed systems run across a lot of servers, but you're only going to be talking to the set responsible for your
files, most of the time.

Imagine that you have 1000 users, and 100 computers, and each computer is responsible for exactly but no more than 10 users.
This is easy to scale up, right? If you add more users, you can always just add more computers. 

While sharding can get a lot more complicated than this - this is the "easy" case, and it works for loads of things.

So, user profile data: you store it by userId, you always search it by userId, it goes to a cluster of computers
responsible for serving information about that userId, you're done. Boom, you made a service that scales horizontally.

What about files? You're sitting on a lot of user file metadata, right? So you store it by userId, you always search it by userId,
it goes to a cluster of computers responsible for serving information about that userId, you're done. Since the files are
stored in S3, which _also works this way_, now you have horizontally scaled file metadata. 

![](./getting_away.gif)

Mongo makes delivering this kind of architecture unbelievably easy.

### DocumentDB and Other Shard-Ish Databases

What is DocumentDB? It's Amazon Mongo. Amazongo?

It's one of a category of "hosted database products" where you can write your code with shards in mind up-front,
and then hand them over to a third party host, and they'll give you access to tools that you can use to convert 
arbitrary amounts of money into arbitrary amounts of More Database. It's expensive, but... 

Well, while you _can_ always vertically and horizontally scale, do _you want to be the poor schmuck on the
hook for planning and executing your database's migration to a new, bigger topology_? 

### If You Are Creative With It, It Is Very Powerful

Sometimes a little bit of denormalization can give you great big powers.

Okay, I'm going to share with you some details of how we manage user Inventories at VRChat, because it's a design I am
legitimately a little bit proud of, even if you're going to think I'm crazy:

User Inventories seem like they're relational data, right?

I mean, if you have Hat X, and someone else has Hat X, that's the same hat, right? 

Wrong! 

Inventories are sharded by user: it's a table with over 100 million entries, but like so many other things, it's bucketable
by userId: 99.9% of the time, I want "every item in Ted's inventory" - I very, very rarely want "every Hat X".

And every single item is a copy of an underlying "Template". There might be 10,000 "Hat X's"- but each of them is a distinct
object, copied from the singular "Hat X" Template. That also means, we can tie per-hat state to Hats - your Hat X can be red, 
and mine can be blue, and that's fine- they're different Hats.  

This makes reads, writes, and sorts within a user inventory incredibly fast, and eternally scalable.

But - when you change Hat X - maybe, renaming it to Hat Y - you might have to do 10,000 writes! 

... Which, it turns out, is okay. Changing an item that people are holding on to? That's _incredibly rare_. 
We don't encourage changing inventory items once they're in users' hands _anyways_. That's an admin operation that should 
_probably_ come with an _announcement_ it's so rare. And that operation **isn't even that slow**: we still have an index on templateId, 
it just means that for a few seconds all of our servers have to focus on that rename, and then they're back to being
incredibly fast at the thing they need to be incredibly fast at: managing individual users' inventories.

### This Would Be A Terrible Architecture if Templates Changed Frequently

Sometimes models like this force you to think through your access patterns in advance: denormalizing items when the items themselves
change frequently would be a disaster: each write multiplied by every copy of the item that exists in the system: and 
a SQL architecture would trade the excellent per-user read/write performance and abysmal template write performance of this scheme 
for "pretty good" performance across both. 

In fact: normalized SQL just... does a pretty good job, most of the time, without a lot of complex tradeoffs.
That's one of the reasons it's _the most popular kind of database_. 

## Anyway: Mongo: It's Pretty Good And If You've Dismissed It Maybe You Shouldn't... Do That, Anymore

Raise a glass for the Mongo fans out there: they're not all cargo-cult magpie idiots. 

This tool can build good, reliable, resilient, scalable, well-thought-out products. 

I think. 