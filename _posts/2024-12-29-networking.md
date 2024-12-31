---
layout: post
title: "Networking and Hardware"
author: "Zach White"
image: network-switch.jpg
---

<center>Image: <a href="https://www.flickr.com/photos/small_realm/15995555571/">Data Center</a> by <a href="https://www.flickr.com/photos/small_realm/">Bob Mical</a>. <a href="https://creativecommons.org/licenses/by-nc/2.0/">CC BY-NC 2.0</a></center>

I always find that the best way to make forward progress is to make forward progress. Start making progress in one direction and you will make progress in other directions as well. I have been finding myself in a lot of circular loops as I try to design this system. Today I'm going to make forward progress by focusing on things that will be usable no matter what software I land on.

Let's start by taking a look at networking. My setup today is pretty pedestrian, only a single subnet for all hosts.

<img src="/assets/img/virtual-network-layout.jpg">

I don't want to change this, but I also don't want to put all my k8s traffic onto that subnet. Clustered storage will be part of this setup, and having a dedicated subnet for that would be good. Back in the day we'd use something like InfiniBand, but I suppose I'll have to settle for Ethernet. That means my new k8s network will look something like this-

<img src="/assets/img/k8s-network-layout.jpg">

Now the question becomes, how do I cleanly integrate this into my current home network?

This is where we pause the networking discussion to change topics. This blog is actually catching up with where I currently am in my homelab exploration. I have already bought and evaluated two ARM SBCs- the Raspberry Pi 5 and the Orange Pi 5 Plus. Both are excellent devices, but after some basic evaluation the choice between them is clear. I will be buying more Orange Pi 5 Pluses to round out my k8s worker nodes.

(You might ask why I skipped over the Intel N100. Remember that this is as much about art as it is practicality, and I have more flexibility to make a cool looking cluster with the SBCs, and as a bonus they use less power. Sometimes form is more important than function.)

Now I know what my worker nodes will be, but what should my Control Plane be? One common strategy is to have a 4 node cluster where 1 node is the Control Plane. But if I'm using Rook Ceph, which is currently leading in my storage investigation, I'll need 4 storage nodes, which means I have to buy a fifth Orange Pi to be the control plane. If only I had a suitable system laying around so I didn't have to spend more money...

If you are particularly eagle eyed, you will have seen the solution already- I have a spare Raspberry Pi 5 now! It will serve as a single node Control Plane, and it can also run something like Traefik or MetalLB, giving me a clean way to connect my cluster to my home network.

If I flesh out my diagram above, it ends up looking like this.

<img src="/assets/img/final-network-layout.jpg">

Great! Now what does an implementation look like? Because we made forward progress above, we can continue to make forward progress here. Specifically, my choice of the Orange Pi 5 Plus means that I will end up buying a 2.5gb switch to go with them. For reasons I'll get into in a future post, I will actually have two switches, one for each vlan. The two NICs on the Orange Pi will come in handy here.

Putting this together into a physical diagram, this is how my network will end up being connected-

<img src="/assets/img/physical-network-layout.jpg">

Next post- Hardware discussion.
