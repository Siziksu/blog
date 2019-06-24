+++
type = "post"
title = "How to Create a Desktop File in Linux"
summary = "This post will show you how to create a desktop file in linux."
date = 2019-06-16T18:58:48+02:00
categories = ["linux"]
tags = ["desktop file", "linux"]
image = "images/chopped-chopped-wood-cut-2203077.jpg"
edit = ""
draft = false
+++
This example will show you how to create a desktop file. For this purpose I will create a desktop file, called `file_name.desktop`, for a program called `program_name` that is installed in the folder `/opt/program_folder/`.

> Each program have a different structure inside its installation folder. You will have know where it is the program itself and the icon.

Lets say that the folder structure for the program in this example is the following:

{{< highlight Plaintext >}}
opt/
└── program_folder/
    ├── program_name.sh
    ├── icon.png
    └── ...
{{< / highlight >}}

<br />

**Steps**

1. Create the `file_name.desktop` file in the folder `~/.local/share/applications/` or `/usr/share/applications/` depending if you want it to be just for your user or for all users.
2. Set the content below in the above file:
{{< highlight Plaintext >}}
[Desktop Entry]
Encoding=UTF-8
Name=program_name
Exec=/opt/program_folder/program_name.sh
Icon=/opt/program_folder/icon.png
Terminal=false
Type=Application
Categories=Development;
{{< / highlight >}}
3. Make the file executable running the following command:
{{< highlight Bash >}}
$ chmod +x file_name.desktop
{{< / highlight >}}

<br />

And that's it.
