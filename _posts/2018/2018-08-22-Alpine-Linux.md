---
layout: post
title: Alpine Linx
categories: [Tips]
tags: [OS, System, Docker]
fullview: false
comments: true
---

If you play with Docker and DevOps, you eventually will hit [alpine linux](https://alpinelinux.org/)

## Why?

* Small: on DockerHub, compare the size on [this page](https://hub.docker.com/r/library/alpine/tags/) and [this page](https://hub.docker.com/r/library/ubuntu/tags/). You will get the feeling of why people love to use Alpine as base image in their Docker image.

* Simple: it has special package management system `apk`, OpenRC init system and script setup. No other daemons, no other services and features. Crystal clear!

* Secure: Kernel is customized with many security features. 

## Limitation

* `apk` system has limited packages, and package version restricted to a particular system version. You could not install higher version package in a lower version system. Package search is [here](https://pkgs.alpinelinux.org/packages)
* The package does not have license information in it. So it is very hard to collect license information if I want to publish my Docker image.

Overall, alpine is a great Linux distribution. I may have more post about it in the future. 