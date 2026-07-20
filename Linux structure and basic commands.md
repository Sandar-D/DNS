# LINUX:
Linux is a family of free and open source (FOSS) operating systems based on a Linux kernel made by Linus Torvalds in 1991. Linus modified the kernel of another operating system called Minix to suit his own needs and released it to the public. In his own words, he originally didn’t make it with the intention of it being FOSS. It was just a piece of software that was open to the public reception, and it was soon noticed by the GNU project which recognized its potential. It evolved from there and as a result today there are over 600 linux distributions for public use alone. 

Although it would be difficult to classify all the Linux distributions, there are outliers. Looking at a wikipedia distro timeline we can see that most linux distributions are derivative of others and as such are separated based on the package manager they use - Slackware based (pkgtools), Debian based (APT), RHEL based (Yum), Arch based (pacman) etc. Most notable distributions include Ubuntu, Fedora, CentOS.

Each distro flavor comes with their own set of preinstalled software which reflects their respective goals and philosophies. For example, Debian is known for stability and support of the FOSS spirit, which is why they use the older version of kernel and offer no official support for proprietary software. Meanwhile Arch is known to be the bleeding edge DIY distro flavor, which is why they support the latest kernel version and expect you to build your own system from scratch using their by far the most extensive documents in Archwiki.

## Linux Structure:
`/` <- root directory stores all the directories in Linux and all other directories are children of root. If you cd into this directory and use the ls command, you will find a list of all the directories on your machine. The absolute path of every file passes through the root directory as it is the parent to all other directories.

`/bin` <- It contains essential system binaries and executables that must be available even if only the partition containing / is mounted. It usually contains the shells like bash or zsh as well as well as commonly used commands like cp, mv, rm, cat, ls. 

`/sbin` <- Contains system binaries that can be executed only by the root user (mount or delete user/group). 

`/lib` <- contains libraries (code) that are shared by the binaries stored in bin and sbin.

`/usr`<- contains all the user binaries, their documentation, libraries etc. User programs like ssh, ftp and so on are placed here. 

`/usr/bin` <- non essential binaries intended for the end-user. 

`/usr/local` <- contains binaries that are compiled manually to prevent conflicts with system installed binaries. 

`/etc` Nerve center of the system, it contains all system related configuration files in here or in its sub-directories. A "configuration file" is defined as a local file used to control the operation of a program; it must be static and cannot be an executable binary.

`/home`<- holds directories registered after each end-user on the system. It contains configuration files for that user and you have to be that user or root to access them. It's accessible with cd ~ sign. 

`/boot` directory stores data that is used before the kernel begins executing user-mode programs. This may include redundant (back-up) master boot records, sector/system map files, the kernel and other important boot files and data that is not directly edited by hand.

`/dev` directory consists of files that represent devices that are attached to the local system. Here we can interact with our partitions as if they were files. 

`/var`<-Contains variable files like logs.

`/tmp`<-used for temporary files that do not persist after each system reboot. 

`/mnt` <-This is a generic mount point under which you mount your filesystems or devices.

`/media`<-This directory contains subdirectories which are used as mount points for
removeable media such as floppy disks, cdroms and zip disks.

`/proc` <- sometimes referred to as a process information pseudo-file system, it doesn't contain 'real' files but rather that is created in memory on the fly by the Linux kernel. It contains a runtime system information like system memory, devices mounted, hardware configuration etc.

## Linux Commands:
`whoami` - displays username of the current user

`man` - manual pages for the commands

`clear` - clears the screen 
	-x <- won't clear scroll history
	
`pwd` - print working directory
	-P <-avoids all symlinks
	
`ls` - list contents of a directory

	-a <- all
	-l <- long listing format
	-h <- human readable
`cd` - change current directory
	../ <- moves us directory above the current one
	~ <- moves us to /home/user directory
	Absolute path is the one that starts from the root of the system
	Relative path is the directory where we are currently and goes deeper into the tree
	
`mkdir` - used to make directories

`touch` - used to create new files
	{1..100} <- make 100 files
	
`rmdir` - removes directory, but works only if it's empty

`rm` - removes both directories and files. Makes rmdir obsolete;
	-f <- forcibly removes everything
	-r <- recursively removes directories and contents. Use to remove directories with content
	-i/I <- prompt before each removal/each 3 removals
	-v <-verbose

`mv` - move/rename files.

`cp` - copy files/folders

`head` - output for the first 10 entries of a file
	-n 100 <- print 100 lines

`tail` - print out the last 10 lines in a file
	-n 100 <- print 100 lines
	-f <- don't stop when the end of the files is reached due to ongoing processes

`>` <- used to redirect standard output in a file 
`>>` <- used to append standard output to a file
`|` <- piping a command into another command

`cat` - concatenates files
	-n <- list a number

`less` - used to read contents of files. Like vim, without text editing.

`echo` - takes an argument we echo and prints it back to us. It's very useful for scripting.

`wc` - used to count words, bytes and lines
	-c <- bytes
	-m <- characters
	-w <- words
	-l <- lines

`sort` - sorts information. Does not change file itself, only the information that is being printed out. It's case sensitive, and uppercase letters take priority in printing.
	-r < sort in reverse order
	-h < human readable sort
	-n < numeric sort
`uniq` - used in conjunction with a sort command, it reports or omits adjacent repeated lines. 
	-u <- removes all repeated lines
	-d <- prints only the duplicates
	-c <-counts the lines

`expansions` - special characters like ~,`*`, ?? (?? means any exactly two characters) etc

`diff` - differences between two different files

`find` - used to find files and folders recursively by a set criteria.

`grep` - finds text inside of the files 

`du` - used to list out the file sizes in a directory
	-s <-summary
	-c <- total
	-h <- human readable
	--max-depth=3 <- How many directories you wish to look, conflicts with s (summary)

`df` displays the amount of disk space available on the file system containing each file name argument.
	-h <-human readable

`history` - keeps memory of all the ran commands

`ps` - process status; inspects processes running on a computer
	 aux < all processes
	 
`htop` - It  is similar to top, but allows you to scroll vertically and horizontally, and interact using a mouse.  You can observe all processes running on  the  system,  along  with their command line arguments, as well as view them in a tree format, select multiple processes and acting on them all at once. Makes ps aux almost obsolete

`gzip` - compresses a file using a gzip compression algorithm. Using a command with just a file name will compress the file and replace the original file with a zipped version. Only zips individual files, even if you use multiple arguments. 
	-k < will keep the original file during zipping process
	-d < will unzipp a zipped file
`gunzip` - unzipps a zipped file. Equivalent to gzip -d

`tar` - used to create an archive, grouping multiple files in a single file
	- c < create
	- f < has to provide a file name 
	- x < extracts archive
	- t < can view what's in the archive
	- z < compresses an archive
	- v < verbose

`alias` - allows us to define our own short custom commands

`xargs` - used for piping. Some commands are made to work with value being piped, but others require argument insertion. xargs is a prefix for those arguments in order to give them values that are being piped as arguments.

`ln` - creates links, similar to a shortcut on windows. Simply, it's a file that points to another file. There are two types, soft links and hard links. 
	Hard link creates a file that stays in sync with another file. It's not a copy or a duplicate, they are pointing to the same exact file in memory. If the original file is removed, hardlink stays.
	Soft link, or symlink (symbolic) is just a shortcut to the file. It can still be read or written to, but if the original file is gone it won't contain additional info.

`lsof` - list all of the currently open files

`netstat and ss` - used to display a list of active network sockets and their associated processes on a Linux system. Here's what each part of the command does:
	- **t**: This option is used to filter and display only TCP sockets.
	- **-l**: This option is used to show listening sockets.
	- **-p**: This option is used to display the process or application that is using the socket.
	- **-n**: This option prevents hostname resolution, displaying numerical addresses instead.
`nohup` - no hang up, like screen, is used to start a background process that won't get interrupted.
`screen` - opens a second terminal screen in order to start an additional process in the background that won't be interrupted
`ping` - sends four packages to the intended server to check the path and activity and reports the outcome
`traceroute` - sends 3 packages to each of the nodes on the path to the intended server and reports on each of the nodes. 

`whois` - CLI tool that chacks if the domain is registered, tells you nameservers from registrar and gives you domain exploration date. It queries whois servers. whois does not perform a dns query and instead it's asking a registrar about the information that can be found in it regarding a domain.

`dig` (domain information groper) is a dns lookup utility that translates readable characters into the ip address that Skynet understands. Unlike whois, dig will perform a dns lookup.
look into dig +trace and dig +norec
	**dig vs whois**
		dig asks local resolver, whois asks whois servers
		dig can query a specific nameeserver, whois asks whois servers
		dig returns DNS records; whois returns registration info

umask
curl
wget
awk
sed



## IPTABLES VS APF

[iptables](https://www.thegeekstuff.com/2011/01/iptables-fundamentals/) is a command-line firewall utility that uses policy chains to allow or block traffic. When a connection tries to establish itself on your system, iptables looks for a rule in its list to match it to. If it doesn't find one, it resorts to the default action.

Tables are a collection of chains that serve a particular function. iptables uses four different tables:  
  
  - `filter` is the default table, and is where all the actions typically associated with a firewall take place.
  - `raw` is used only for configuring packets so that they are exempt from connection tracking.
  - `nat` is used for [network address translation](https://en.wikipedia.org/wiki/Network_address_translation "wikipedia:Network address translation") (e.g. port forwarding).
  - `mangle` is used for specialized packet alterations.

Iptables’s filter table has the following built-in chains.
  - **Input** - This chain is used to control the behavior for incoming connections. For example, if a user attempts to SSH into your PC/server, iptables will attempt to match the IP address and port to a rule in the input chain.
  - **Forward** - This chain is used for incoming connections that aren't actually being delivered locally. Think of a router - data is always being sent to it but rarely actually destined for the router itself; the data is just forwarded to its target. Unless you're doing some kind of routing, NATing, or something else on your system that requires forwarding, you won't use this chain.
  - **Output** - This chain is used for outgoing connections. For example, if you try to ping howtogeek.com, iptables will check its output chain to see what the rules are regarding ping and howtogeek.com before making a decision to allow or deny the connection attempt.

## IPTABLES RULES
- Rules contain a criteria and a target.
- If the criteria is matched, it goes to the rules specified in the target (or) executes the special values mentioned in the target.
- If the criteria is not matached, it moves on to the next rule.

### Target Values

Following are the possible special values that you can specify in the target.
- ACCEPT – Firewall will accept the packet.
- REJECT - Firewall will reject the packet but notify the user it was dropped.
- DROP – Firewall will drop the packet.
- QUEUE – Firewall will pass the packet to the userspace.
- RETURN – Firewall will stop executing the next set of rules in the current chain for this packet. The control will be returned to the calling chain.

The rules in the iptables –list command output contains the following fields:
- num – Rule number within the particular chain
- target – Special target variable that we discussed above
- prot – Protocols. tcp, udp, icmp, etc.,
- opt – Special options for that specific rule.
- source – Source ip-address of the packet
- destination – Destination ip-address for the packet

To list out the iptables:
```
root@nova:/home/sandar# iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination         
root@nova:/home/sandar# apf
```

### APF

The configuration of APF is designed to be very informative and present the user with an easy to follow process, from top to bottom of the configuration file. The management of APF on a day-to-day basis is conducted from the command line with the 'apf' command,
which includes detailed usage information and all the features one would expect from a current and forward thinking firewall solution.

The technical side of APF is such that it embraces the latest stable features put forward by the iptables (netfilter) project to provide a very robust and
powerful firewall. The filtering performed by APF is three fold:
1. Static rule based policies (not to be confused with a "static firewall")
2. Connection based stateful policies
3. Sanity based policies

The first, static rule based policies, is the most traditional method of
firewalling. This is when the firewall has an unchanging set of instructions (rules) on how traffic should be handled in certain conditions. An example of a static rule based policy would be when you allow/deny an address access to the server with the trust system or open a new port with conf.apf. So the short of it is rules that infrequently or never change while the firewall is running.

The second, connection based stateful policies, is a means to distinguish legitimate packets for different types of connections. Only packets matching a known connection will be allowed by the firewall; others will be rejected. An example of this would be FTP data transfers, in an older era of firewalling you would have to define a complex set of static policies to allow FTA data transfers to flow without a problem. That is not so with stateful policies, the firewall can see that an address has established a connection to port 21
then "relate" that address to the data transfer portion of the connection and dynamically alter the firewall to allow the traffic.

The third, sanity based policies, is the ability of the firewall to match various traffic patterns to known attack methods or scrutinize traffic to conform to Internet standards. An example of this would be when a would-be attacker attempts to forge the source IP address of data they are sending to you, APF can simply discard this traffic or optionally log it then discard it. To the same extent another example would be when a broken router on the Internet begins to relay malformed packets to you, APF can simply discard them or in other situations reply to the router and have it stop sending you new packets (TCP Reset).


To list out apf (it will open nano):
```
apf -l
Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
num   pkts bytes target     prot opt in     out     source               destination

Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
num   pkts bytes target     prot opt in     out     source               destination

Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)
num   pkts bytes target     prot opt in     out     source               destination
```

Inodes contain metadata about a file system. They contain every piece of information regarding a file - size of a file, who owns it, permission string etc. It contains all the metadata besides the file name and file content.

Every storage medium has its own set of inodes.