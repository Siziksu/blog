+++
type = "post"
title = "Backing up and restoring the Linux configuration"
summary = "This post will explain how to backup the Linux configuration (dconf keys)."
date = 2019-06-09T12:56:50+02:00
categories = ["linux"]
tags = ["dconf", "backup", "linux"]
image = "images/architecture-building-contemporary-258846.jpg"
edit = 2019-06-16T13:08:48+02:00
draft = false
+++
Linux stores the configuration and settings in a `dconf` database that consists of a single file in binary format. The format is defined as `gvdb` (GVariant Database file). It is a simple database file format that stores a mapping from strings to GVariant values in a way that is extremely efficient for lookups.

The GNOME database file for each user is by default `~/.config/dconf/user`. To explore/edit the `dconf` database, you can use the `dconf Editor`.

**To backup:**

From a terminal, run:

`$ dconf dump /org/cinnamon/ > org_cinnamon.dconf`

This will create the file `org_cinnamon.dconf` in the folder where you are running the terminal.

**To restore all your settings:**

`$ dconf load /org/cinnamon/ < org_cinnamon.dconf`

> Note, cinnamon may freeze crash after this (recommend at least logging out/back in).

**To reset to defaults:**

`$ dconf reset -f /org/cinnamon/`

> Note, cinnamon may freeze or crash doing this.

**Usefull keys to backup:**

{{< highlight Bash >}}
# Terminal configuration
$ dconf dump /org/gnome/terminal/legacy/ > org_gnome_terminal_legacy.dconf

# System Keybindings configuration
$ dconf dump /org/cinnamon/desktop/keybindings/ > org_cinnamon_desktop_keybindings.dconf

# Nemo configuration
$ dconf dump /org/nemo/ > org_nemo.dconf
{{< / highlight >}}
