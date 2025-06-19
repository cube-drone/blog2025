+++
title = 'async traits in rust'
date = 2025-01-21T09:00:00-07:00
draft = false
categories = ["software", "rust"]
tags = []
+++

you know Godel's Incompleteness Theorem? The idea that any consistent formal system is incapable of proving all truths?

I think that I believe that something similar exists for formal type systems: for each of them there are concepts that are useful but inexpressible

anyways Rust's type system has conspired to make something that seems like it should be simple: async functions in object traits - seemingly impossible, and apparently this remains an unsolved probelm

https://smallcultfollowing.com/babysteps/blog/2019/10/26/async-fn-in-traits-are-hard/

> Support for async fn in traits is probably the single most common feature request that I hear about. It’s also one of the more complex topics. So I thought it’d be nice to do a blog post kind of giving the “lay of the land” on that feature – what makes it complicated? What questions remain open?

uh, suffice to say, the fine details of this intelligent strangers' blog entry mostly go above my tiny peanut head and I just read "it's stupendously difficult for a lot of complex type safety reasons, and there's a macro that makes it possible using devil magic"

In general, this seemingly benign idea: "i'm going to define my service interfaces as traits and then I can implement the backend using swappable services that each implement the trait"

has felt like a bit of an uphill battle in Rustland