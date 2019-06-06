---
title: "Linux locations"
date: 2019-06-03T21:30:50+02:00
draft: false
image: "images/architecture-bridge-buildings-1168940.jpg"
tags: [linux,locations]
categories: [linux]
---
# Linux locations

One of the things that we always need to know when working with an operating system, is where the things are.

## Repositories

The repositories are located in the folder `/etc/apt/sources.list.d/`.

## Path

The `$PATH` is set in the file `/etc/environment`.

## Desktop files

The `*.desktop` files are located in the folders `~/.local/share/applications/` and `/usr/share/applications/`.

## Software installation folder

If you are installing software manually, for instance a software downloaded as a `tar.gz` file, should be installed in the folder `/opt/`.
