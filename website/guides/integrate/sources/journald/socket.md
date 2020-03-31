---
last_modified_on: "2020-03-31"
$schema: "/.meta/.schemas/guides.json"
title: "Send logs from Journald to a TCP, UDP, or UDS socket"
description: "A simple guide to send logs from Journald to a TCP, UDP, or UDS socket in just a few minutes."
author_github: https://github.com/binarylogic
cover_label: "Journald to Socket Integration"
tags: ["type: tutorial","domain: sources","domain: sinks","source: journald","sink: socket"]
hide_pagination: true
---

import ConfigExample from '@site/src/components/ConfigExample';
import DaemonDiagram from '@site/src/components/DaemonDiagram';
import InstallationCommand from '@site/src/components/InstallationCommand';
import Jump from '@site/src/components/Jump';
import Steps from '@site/src/components/Steps';

Logs are an _essential_ part of observing any
service; without them you are flying blind. But collecting and analyzing them
can be a real challenge -- especially at scale. Not only do you need to solve
the basic task of collecting your logs, but you must do it
in a reliable, performant, and robust manner. Nothing is more frustrating than
having your logs pipeline fall on it's face during an
outage, or even worse, disrupt more important services!

Fear not! In this guide we'll show you how to send send logs from [Journald][urls.journald] to [a TCP, UDP, or UDS socket][urls.socket]
and build a logs pipeline that will be the backbone of
your observability strategy.

<!--
     THIS FILE IS AUTOGENERATED!

     To make changes please edit the template located at:

     website/guides/integrate/sources/journald/socket.md.erb
-->

## Background

### What is Journald?

[Journald][urls.journald] is a utility for accessing log data across a variety of system services. It was introduce with [Systemd][urls.systemd] to help system administrator collect, access, and route log data.

## Strategy

### How This Guide Works

We'll be using [Vector][urls.vector_website] to accomplish this task. Vector
is a [popular][urls.vector_stars], lightweight, and
[ultra-fast][urls.vector_performance] utility for building observability
pipelines. It's written in [Rust][urls.rust], making it memory safe and
reliable. We'll be deploying Vector as a
[daemon][docs.strategies#daemon].

The [daemon deployment strategy][docs.strategies#daemon] is designed for data
collection on a single host. Vector runs in the background, in its own process,
collecting _all_ data for that host.
For this guide, Vector will collect data from
Journald via Vector's
[`journald`][docs.sources.journald].
The following diagram demonstrates how it works.

<DaemonDiagram
  platformName={null}
  sourceName={"journald"}
  sinkName={"socket"} />

### What We'll Accomplish

To be clear, here's everything we'll accomplish in this short guide:

<ol className="list--checks list--flush">
  <li>
    Collect Journald/Systemd logs.
    <ol>
      <li>Filter which Systemd units you collect them from.</li>
      <li>Checkpoint your position to ensure data is not lost between restarts.</li>
      <li>Enrich your logs with useful Systemd context.</li>
    </ol>
  </li>
  <li>
    Stream logs over a TCP, UDP, or Unix socket.
    <ol>
      <li>Buffer your data in-memory or on-disk for performance and durability.</li>
    </ol>
  </li>
  <li className="list--li--arrow list--li--pink text--bold">All in just a few minutes!</li>
</ol>

## Tutorial

<Steps headingDepth={3}>
<ol>
<li>

### Install Vector

<InstallationCommand />

</li>
<li>

### Configure Vector

<ConfigExample
  format="toml"
  path="vector.toml"
  sourceName={"journald"}
  sinkName={"socket"} />

</li>
<li>

### Start Vector

```bash
vector --config vector.toml
```

That's it! Simple and to the point. Hit `ctrl+c` to exit.

</li>
</ol>
</Steps>

## Next Steps

Vector is _powerful_ utility and we're just scratching the surface in this
guide. Here are a few pages we recommend that demonstrate the power and
flexibility of Vector:

<Jump to="https://github.com/timberio/vector" leftIcon="github" target="_blank">
  <div className="title">Vector Github repo <span className="badge badge--primary"><i className="feather icon-star"></i> 4k</span></div>
  <div className="sub-title">Vector is free and open-source!</div>
</Jump>

<Jump to="/guides/getting-started/" leftIcon="book">
  <div className="title">Vector getting started series</div>
  <div className="sub-title">Go from zero to production in under 10 minutes!</div>
</Jump>

<Jump to="/guides/getting-started/" leftIcon="book">
  <div className="title">Vector documentation</div>
  <div className="sub-title">Thoughtful, detailed docs that respect your time.</div>
</Jump>


[docs.sources.journald]: /docs/reference/sources/journald/
[docs.strategies#daemon]: /docs/setup/deployment/strategies/#daemon
[urls.journald]: https://www.freedesktop.org/software/systemd/man/systemd-journald.service.html
[urls.rust]: https://www.rust-lang.org/
[urls.socket]: https://en.wikipedia.org/wiki/Network_socket
[urls.systemd]: https://systemd.io/
[urls.vector_performance]: https://vector.dev/#performance
[urls.vector_stars]: https://github.com/timberio/vector/stargazers
[urls.vector_website]: https://vector.dev