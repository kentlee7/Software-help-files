---
undefined: ""
---
2024.07.03

I recently found out about Obsidian, which is a great app for taking and organizing notes on all kinds of things. It's great for saving links, taking notes on online courses that I'm doing, and writing drafts of articles. 

Recently, while trying to open Obsidian on a fresh install of PopOS Linux, I got the following error message:

`Error: EMFILE: too many open files`

Apparently I reached some kind of limit -- I'm not sure what, as I just started using Obsidian a few weeks ago. A Google search led me to one fix that actually worked, which involves increasing a limit on Linux inodes (which function as operating system file indices). 

Anyway, here is the solution that fixed my Obsidian. This should work for any version of Ubuntu based systems.

`sudo nano /etc/sysctl.conf  # (apparently other /etc/sysctl.d/\*.conf files supposedly work)`

At the end of the file, add these lines:

`fs.inotify.max_user_watches = 524288`
`fs.inotify.max_user_instances = 1024` 

Exit, and then execute the following:

`sudo systcl --system   # Causes the kernel to reread the system.conf files`

And that was it. Obsidian opened like normal after that. 

I'm not sure why I encountered this error, as I only have a few dozen files in my Obsidian directory. Maybe because it's in my Dropbox folder, and Dropbox sometimes gets persnickety about a lot of files in a directory, at least in Linux. Or maybe it's because I have too many files on my desktop. 

By the way, a bonus tip. as I mentioned, my Obsidian files are in my Dropbox (in a folder called 'Obsidian'), and that is a great way to easily sync my Obisidan notes across multiple devices. 