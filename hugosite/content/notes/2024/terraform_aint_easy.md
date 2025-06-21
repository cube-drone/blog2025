+++
title = 'terraform aint easy'
date = 2024-02-13T10:00:00-07:00
draft = false
categories = ["software"]
tags = ["devops", "terraform", "clickops", "iac"]
+++

There was a recent Reddit post where a DevOps employee lost their shit because their co-workers weren't getting on board immediately with their Terraform repo, and

I'm gonna say it: despite ClickOps' many problems, Terraform has a bad UI and ClickOps has a good UI, getting people who are used to being able to do things with a few clicks to read a stack of manuals is going to be a challenge, especially if those people have "other things to do with their time".

Having an organization where everybody makes changes to infrastructure with PRs and a papertrail seems like it's obviously gonna be better but affecting that particular change by simply creating the repo isn't gonna do it.

You're gonna have to make the Terraform repo the ONLY way to change infra, and for a good while you're gonna have to deal with going from an org where everybody can make changes to an org where everything is backed up by _only one person knowing Terraform_.

Like, don't confuse this for me saying "infrastructure as code is bad", it's good, and for any org with more than a handful of people making cloud changes it's gonna be obviously better, but the effort to switch over to Terraform, which is hard, from ClickOps, which is easy, is significant.