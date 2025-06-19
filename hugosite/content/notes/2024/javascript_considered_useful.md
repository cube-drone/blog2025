+++
title = 'javascript and mongo: good actually'
date = 2024-12-13T12:00:00-07:00
draft = false
categories = ["software"]
tags = ["javascript", "mongodb"]
+++

part of my job remains explaining to people that MongoDB and JavaScript are good, actually

a thing that, after many years, I well and truly believe, now

no, really! they're good! there have been instances in the past of them not being good but they actually can be used to build very impressive services at scale!

you have to learn to write MongoDB like you would Cassandra/Scylla/Dynamo a little bit, though

the thing you lose going to Mongo from a SQL database is "joins", but - if you're building a system that needs to write-scale, you're not going to be able to use joins anyways, joins won't work across shards, so maybe you should be considering "only application side joins and as few of them as possible" from the get-go anyways

of course, "building with scale in mind from the outset" is a bad strategy for most products because honestly:
the chance any given product is going to NEED to scale that big is like 1/1000, most companies literally just need POSTGRES and then SQL already has decades of lore and understanding and tooling behind it

_use postgres, dummy_

but the fact that we started with Mongo and built with Mongo and now have a product that's on Mongo is not a bad thing, and is, in fact, better than if we had a hugely interconnected SQL database that we had to try to unknot every time write performance against a table became a problem

each individual Mongo collection is an utterly self-contained entity that can live on a completely different server, if necessary

did you know you can just take a problem table and move it to its whole own cluster? it's expensive but try and do that with something that has a load of joins pointed at it and you'll discover why that's not often a viable strategy in SQL land

and you know what? Mongo's schemaless, flexible design and easy post-hoc application of indexes to remedy slow queries? that creates a pretty buttery smooth experience to develop an experimental new product with.

In lots of systems, you don't need a load of joins anyways: do you really ACTUALLY have a lot of relationships or does everything just have the single back-link to a "userId"?

In many cases, you have nested relationships, like a TodoGroup filled with Todos, but

_wait a minute, that can just be a nested object, sharded by userId._

As for JavaScript, most of the haters are hating on the JavaScript of 2004 and a couple of NaN memes they saw once. Digging in nowadays, JS is, IMO, equivalent in its power and expressiveness to Python, but with a much clearer model for cooperative multitasking. Not a lot of folks out there hating on Python.

“Oh, template based inheritance is bad” yeah, well, so are deeply nested inheritance hierarchies which are basically impossible to build in JS thanks to its comparatively slim object model.

I will say that I’m not in love with TypeScript: if you want strict, static typing there are languages that do it way, way better, why not just use one of those? C#, or Rust, or Kotlin, or hell, just admit you’re a Java developer.