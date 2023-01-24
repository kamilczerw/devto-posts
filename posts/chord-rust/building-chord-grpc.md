---
title: Building a Chord Ring with Rust and gRPC
description: 'Continuation of the guide about building the Chord protocol in Rust, this time with gRPC.'
tags: 'rust,tutorial,programming,grpc'
cover_image: ./images/cover.png
canonical_url: null
published: false
id: 1337834
---

This is a part 2 of my journey of building a Chord ring in Rust. In the first part, I covered the basics of the Chord protocol and implemented it in Rust. In this part, I will be adding gRPC to the project to make it easier to communicate between nodes. I will also be adding a CLI to the project to make it easier to interact with the nodes.

If you haven’t read the first part, I recommend reading it first. You can find it here: [Building a Chord Ring in Rust](./building-chord-part-1.md).

All the code can be found in the repo https://github.com/kamilczerw/chord. There is a `grpc` branch that contains the code from this post.

## gRPC

>> TODO: Add a description of gRPC

## Adding gRPC to the project

All the code related to gRPC will be in the `libs/grpc` module. Let's start by creating a new module:

```bash
cargo new libs/grpc --lib
```

I'm going to use the [tonic](https://github.com/hyperium/tonic) library to implement gRPC. 

Let's add it to the dependencies:

```bash
cargo add --package grpc tonic
```

Now, let's create a new file `libs/grpc/proto/chord.proto` and add the following code:

```proto
syntax = "proto3";

package chord;

service Chord {
  rpc FindSuccessor (FindSuccessorRequest) returns (FindSuccessorResponse);
  rpc GetPredecessor (GetPredecessorRequest) returns (GetPredecessorResponse);
  rpc Notify (NotifyRequest) returns (NotifyResponse);
  rpc Ping (PingRequest) returns (PingResponse);
}

enum IpVersion {
  IPV4 = 0;
  IPV6 = 1;
}

message IpAddress {
  IpVersion version = 1;
  bytes address = 2;
}

message Node {
  IpAddress ip = 1;
  int32 port = 2;
}

message FindSuccessorRequest {
  int64 id = 1;
}

message FindSuccessorResponse {
  int64 id = 1;
  Node node = 2;
}

message GetPredecessorRequest {
}

message GetPredecessorResponse {
  optional Node node = 1;
}

message NotifyRequest {
  Node node = 1;
}

message NotifyResponse {
}

message PingRequest {
}

message PingResponse {
}
```

We need to install the `protoc` compiler to generate the Rust code from the proto file. You can find the installation instructions [here](https://grpc.io/docs/protoc-installation/).

To generate the Rust code, we need to install the `tonic-build` crate:

```bash
cargo add --package grpc tonic-build --build
cargo add --package grpc prost
``` 

Now, we can add the following code to the `build.rs` file:

```rust
fn main() -> Result<(), Box<dyn std::error::Error>> {
    tonic_build::compile_protos("proto/chord.proto")?;
    Ok(())
}
```

```
chord $ cargo add --package server tokio --features "rt-multi-thread"
```

