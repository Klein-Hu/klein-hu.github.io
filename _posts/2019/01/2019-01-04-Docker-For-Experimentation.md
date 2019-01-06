---
layout: post
title: Docker for Experimentation
categories: [Tips, Docker]
tags: [Docker]
fullview: false
comments: true
---

Docker is more and more used in microservice, devops and even in IoT. I don't think I need to mention those area any more. But it could do more.

I start having this feeling when I try to play with [GRPC](https://grpc.io/). In its [C++ build instruction](https://github.com/grpc/grpc/tree/master/src/cpp#make), the below paragraph scared me:

```
WARNING: After installing with make install there is no easy way to uninstall, which can cause issues if you later want to remove the grpc and/or protobuf installation or upgrade to a newer version.
```

I could not blame anyone if I install gRPC failed due to my own misktake. But even install correctly, the current installation will block my UPGRADE to a new version. This is not something right. I could track what is going to be changed during installation but apparently it may not work properly when I try to use this log to uninstall the current gRPC library.

There is another situation is when I try to build my own GNU nano editor locally some task. The build need to install lots of libraries which most likely will be used only by this particular build and may introduce conflict later with other software I will install in the PC. How to setup an environment just for this build? Install Linux on a new box? Create a VM? Those solutios seem too heavy to me.

The way I could do it create a Dockerfile locally, let the Docker build install all necessary libraries in different layers, which means when I run Docker build again, originally installed libraries do not need to be installed again. This is just different from what we do for microservices trying to install all packages into one layer. But it is useful especially when you are trying to figure out which libraries are needed.

After dependencies installed, there are two ways to run the build:

1. Download/clone code in the image and run the build;
2. Map a local folder into the container when it is running. 

Both way could work but the second way generated code in the mapping folder will be owned by root. This needs to be taken care later when use them.

Here is a sample of Dockerfile to build nano. You could find it in [Gist](https://gist.github.com/Klein-Hu/acc4fb36597bf68f10adaebc28a80610) as well.

```Dockerfile
FROM ubuntu:18.04

RUN apt-get update
RUN apt-get install -y git build-essential autoconf libtool pkg-config libncursesw5 libncursesw5-dev ncurses-doc groff texinfo autopoint gettext

WORKDIR /code
RUN git clone git://git.savannah.gnu.org/nano.git nano

WORKDIR /code/nano
RUN ./autogen.sh
RUN ./configure
RUN make
```
