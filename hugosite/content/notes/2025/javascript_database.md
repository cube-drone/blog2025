+++
title = 'an in-client database'
date = 2025-03-15T12:00:00-07:00
draft = false
categories = ["software"]
tags = ["javascript"]
+++

about once every ten thousand years, the prophecy states a frontend developer will realize that they spend most of their time carefully using RPCs to curate a little miniature in-client database in order to keep their UI data up-to-date and sane,

and, in an effort to solve this problem for themselves in perpetuity, end up nerd sniping themselves for multiple consecutive years on a synchronization or complex query solution to perma-solve this problem, like Meteor.js or GraphQL

These solutions tend to be so powerful that they make simple applications like the well known Todo App seem almost magically trivial

but the black-box, complex and highly bespoke nature of these solutions keeps them niche: RPC over HTTP is a well-understood model for a reason, it's much harder to reason about access control or rate limiting in a Custom Sync Protocol made by Some Guy

it's even worse when, once every ten thousand years, a backend developer decides that they will refuse to learn Javascript and end up inventing some fabulously, wildly elaborate system to write everything in a unified environment

like ASP.NET WebForms or Elixir's Phoenix Live Views