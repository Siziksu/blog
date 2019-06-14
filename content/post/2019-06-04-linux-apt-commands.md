+++
title = "Linux 'apt' commands"
description = "This post will have a look at the Linux 'apt' and 'apt-get' package tools."
date = 2019-06-04T21:30:50+02:00
categories = ["linux"]
tags = ["linux","apt"]
image = "images/adorable-animal-canine-1254140.jpg"
edit = ""
draft = false
+++
There are two package tools, `apt` and `apt-get`. `apt` is just the rework of `apt-get`.

{{< highlight Plaintext >}}
apt install       apt-get install       Installs a package
apt remove        apt-get remove        Removes a package
apt purge         apt-get purge         Removes package with configuration
apt update        apt-get update        Updates the database of the packages
apt upgrade       apt-get upgrade       Upgrades all upgradeable packages
apt autoremove	  apt-get autoremove    Removes unwanted packages
apt full-upgrade  apt-get dist-upgrade  Upgrades packages with auto-handling of dependencies
apt search        apt-cache search      Searches for packages
apt show          apt-cache show        Shows package details
apt list                                Lists packages with criteria (installed, upgradeable etc)
apt edit-sources                        Edits sources list
{{< / highlight >}}

**Install**

If you run `apt install` on an already installed package, will just look into the database and,
if a newer version is found, it will upgrade the installed package to the newer one.

{{< highlight Bash >}}
$ sudo apt install <package_name>
{{< / highlight >}}

If you don't want to upgrade a package already installed when you use the install, you can use the option `–no-upgrade`.

{{< highlight Bash >}}
$ sudo apt install <package_name> --no-upgrade
{{< / highlight >}}

Also, you can install an specific version of a package.

{{< highlight Bash >}}
$ sudo apt install <package_name>=<version_number>
{{< / highlight >}}

**Upgrade**

{{< highlight Bash >}}
$ sudo apt update       # Updates the repositories
$ sudo apt upgrade      # If there is any upgradeable repository, upgrades it
$ sudo apt full-upgrade # Upgrades packages with auto-handling of dependencies
{{< / highlight >}}

> If you have package version 1.3 installed, after `apt update`, the database will be aware that a newer version 1.4 is available.
When you do an `apt upgrade` after `apt update`, it upgrades the installed packages to the newer version.

{{< highlight Bash >}}
$ sudo apt update && sudo apt upgrade -y # This line, performs an automatic (-y) update and upgrade
{{< / highlight >}}

**Remove**

{{< highlight Bash >}}
$ sudo apt remove <package_name> # Removes just the package
{{< / highlight >}}

**Purge**

{{< highlight Bash >}}
$ sudo apt purge <package_name> # Removes a package and configuration
{{< / highlight >}}

**Search**

{{< highlight Bash >}}
$ sudo apt search <package_name> # Searches for packages
{{< / highlight >}}

**Show**

{{< highlight Bash >}}
$ sudo apt show <package_name> # Shows package details
{{< / highlight >}}

**Clean**

{{< highlight Bash >}}
$ sudo apt autoremove # Removes unwanted packages
{{< / highlight >}}
