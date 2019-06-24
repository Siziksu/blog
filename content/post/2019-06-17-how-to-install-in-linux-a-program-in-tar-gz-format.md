+++
type = "post"
title = "How to install in Linux a program in tar.gz format"
summary = "In this post I'll show you how to install in Linux a program that you downloaded as a tar.gz file."
date = 2019-06-17T19:52:46+02:00
categories = ["linux"]
tags = ["tar-gz", "install", "linux"]
image = "images/1-wtc-america-architecture-374710.jpg"
edit = ""
draft = false
+++
Well, sometimes you want to install a program in Linux, but the only download option that you have is a compressed file like `tar.gz`. Lets say, in this case, that the downloaded file is called `file-downloaded.tar.gz`.

First, extract the files with the command `$ tar -xvzf file-downloaded.tar.gz`, and lets say that this command extracts your file into a folder called `program_folder`.

> Sometimes you'll find instructions about how to install the program in the extracted folder. If there are no instructions on how to install the program in the folder you already extracted, then keep on reading this post, otherwise follow that instructions.

If you are still reading this, is because in your extracted folder there are no instructions to guide you through the process. So, the next step will be to move the extracted folder to the installation folder.

The directory `/opt` is reserved for all the software and add-on packages that are not part of the default installation, so, we will execute the commmand `$ sudo mv program_folder /opt`. This will move the folder `program_folder` into the `/opt` folder.

> Moving to the folder `/opt` folder requires permission, that is why we are using `sudo`.

The last step will be to create a `desktop file` to be able to run the program. For this step, I already did another post on how to do it:

<br />

[How to Create a Desktop File in Linux](../how-to-create-a-desktop-file-in-linux/)

<br />

And that's it!
