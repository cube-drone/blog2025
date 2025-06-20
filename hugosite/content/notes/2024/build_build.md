+++
title = 'build'
date = 2024-05-01T12:00:00-07:00
draft = false
categories = ["software", "humor"]
tags = ["devops"]
+++

today in work:

a build with "build" in the name of the build will build, but it won't deploy
because when we build it, it includes "build" in the name of the completed build

but when we try to deploy it, it decides that "build" is the cut-off point for the name of the build, but it uses the "build" in the name of the build rather than the "build" added by a completed build: as a result it can't find the correct build

in conclusion: build