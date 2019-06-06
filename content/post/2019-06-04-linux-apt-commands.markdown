---
title: "Linux apt commands"
date: 2019-06-04T21:30:50+02:00
draft: false
image: "images/adorable-animal-canine-1254140.jpg"
tags: [linux,apt]
categories: [linux]
---
# Linux apt commands

## Commands

There are two package tools, `apt` and `apt-get`.

{{< highlight Text >}}
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

## Install

If you run `apt install` on an already installed package, will just look into the database and,
if a newer version is found, it will upgrade the installed package to the newer one.

{{< highlight Text >}}
$ sudo apt install <package_name>
{{< / highlight >}}

If you don't want to upgrade a package already installed when you use the install, you can use the option `–no-upgrade`.

{{< highlight Text >}}
$ sudo apt install <package_name> --no-upgrade
{{< / highlight >}}

Also, you can install an specific version of a package.

{{< highlight Text >}}
$ sudo apt install <package_name>=<version_number>
{{< / highlight >}}

## Upgrade

{{< highlight Text >}}
$ sudo apt update       # Updates the repositories
$ sudo apt upgrade      # If there is any upgradeable repository, upgrades it
$ sudo apt full-upgrade # Upgrades packages with auto-handling of dependencies
{{< / highlight >}}

> If you have package version 1.3 installed, after `apt update`, the database will be aware that a newer version 1.4 is available.
When you do an `apt upgrade` after `apt update`, it upgrades the installed packages to the newer version.

{{< highlight Text >}}
$ sudo apt update && sudo apt upgrade -y # This line, performs an automatic (-y) update and upgrade
{{< / highlight >}}

## Remove

{{< highlight Text >}}
$ sudo apt remove <package_name> # Removes just the package
{{< / highlight >}}

## Purge

{{< highlight Text >}}
$ sudo apt purge <package_name> # Removes a package and configuration
$ sudo apt remove --purge <package_name>
{{< / highlight >}}

## Search

{{< highlight Text >}}
$ sudo apt search <package_name> # Searches for packages
{{< / highlight >}}

## Show

{{< highlight Text >}}
$ sudo apt show <package_name> # Shows package details
{{< / highlight >}}

## Clean

{{< highlight Text >}}
$ sudo apt autoremove # Removes unwanted packages
{{< / highlight >}}
