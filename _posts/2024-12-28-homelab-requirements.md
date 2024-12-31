---
layout: post
title: "Homelab Requirements"
author: "Zach White"
image: centaur-server-room.jpg
---

<center>Image: <a href="https://www.flickr.com/photos/viagallery/2293424530/">Centaur server room</a> by <a href="https://www.flickr.com/photos/viagallery/">VIA Gallery</a>. <a href="https://creativecommons.org/licenses/by/2.0/deed.en">CC BY 2.0</a></center>

Before I get too deep into building a Homelab, I should set out some requirements and a budget. Let's start by thinking about what it is I want to accomplish here.

First and foremost, I want to learn more about Kubernetes, and specifically the CNCF things we use at work. In a practical sense, this makes some choices for me. I want to follow industry standards and recommendations as well, which means I will need to account for Control Plane nodes that are separate from my Workload nodes. I would also like to avoid virtualizing my k8s nodes, because I feel that containers are virtualization enough. This means I will need 4 physical machines at minimum- 1 Control Plane, 3 Workload. Having a single Control Plane node leaves me with a SPOF, but this is just a homelab. 🤷

## Workloads

My planned workload for this infrastructure revolves around my home automation needs. Currently I run a handful of apps on containers, but they are all stand-alone and have few dependencies between them. I use MQTT and Homebridge to tie these disparate pieces together into a coherent whole. It would be nice to move everything into a reliable Kubernetes homelab setup, because right now my monitoring is pretty poor. I also do not do any video storage or processing, and it would be nice if I could use some of the AI detection stuff.

## Storage

I do not currently do much storage, but I would like to start. One of my future projects is to put cameras up around the house, and use AI to do person and object detection. It would also be nice to self host what I currently use Dropbox for, and maybe have a little more space available.

I would like this storage to be redundant, and to be some form of cluster storage. I don't want to build a NAS and a cluster, I just want to build a cluster. If I can add more compute and storage by adding physical k8s nodes, that's exactly where I want to be. Rook Ceph looks like an interesting possibility here...

## Physical and Electrical Resiliency

Since my primary workload is home automation, running it when the internet is offline will be important. I live in a place that sees several power outages each year, so my homelab will need some way to survive those outages. It might be nice if my homelab ran mostly or completely on solar power. Low power consumption will be important in that situation, and even if I use grid power it can be expensive, so I will still want low power consumption. For low power consumption there are a lot of choices in ARM Single Board Computers. Intel has made a lot of progress with the N100, and I will look at that too, but my gut says that ARM will be the winner.

## Art

Going beyond pure technical requirements, this is also an art project. Non-technical people should have something they can look at to get a sense of what is going on. I want people who have the right background to understand how cool this actually is. With my skills as a maker, designing cases and custom parts for this Homelab is a possibility. Designing a completely custom case for this cluster might be a fun challenge.

## Budget

Since I'm buying hardware, there's going to be a capex associated with this. Some back of the napkin math says I'll need 4 physical nodes, and those will probably run me $200-300 each, starting me at $1000. I'll need a network switch, and hardware to bring it all together in a nice package too. As a starting point I'm going to set a target budget of $2000, with a max budget of $3000 if I can justify the extra expenses.

This is not a small amount of money, but it's also not unreasonable when looking at hobby costs. I've had many hobbies where I spent that much, or more, over the course of a year. In this case I have to spend most of it upfront, and that concentrates the pain. However, I am confident I can make something cool if I allow myself the freedom to spend. Setting a budget now will help me avoid abusing that freedom.

## Distilled Requirements

Boiling the above down into a list of requirements-

* 4 physical k8s nodes
* Fast local storage for containers
* Fast(ish) local storage for clustered FS

It looks like I want something that either has 4 NVMe slots, or 1-2 NVMe and 2 SATA. That will give me 1-2 drives for the system drive and 2 drives for the clustered FS.
