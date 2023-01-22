---
title: 'Building a Chord Ring with Rust and gRPC'
description: Continuation of the guide about building the Chord protocol in Rust, this time with gRPC.
tags: 'rust,tutorial,programming,grpc'
cover_image: ./images/cover.png
canonical_url: null
published: false
---

This is a part 2 of my journey of building a Chord ring in Rust. In the first part, I covered the basics of the Chord protocol and implemented it in Rust. In this part, I will be adding gRPC to the project to make it easier to communicate between nodes. I will also be adding a CLI to the project to make it easier to interact with the nodes.

If you haven’t read the first part, I recommend reading it first. You can find it here: [Building a Chord Ring in Rust](./building-chord-part-1.md).

All the code can be found in the repo https://github.com/kamilczerw/chord. There is a `grpc` branch that contains the code from this post.
