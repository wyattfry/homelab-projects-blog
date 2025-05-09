---
date: '2025-05-09T09:58:00-04:00'
draft: false
title: 'Enshrouded Dedicated Server'
---

![A cat reading a newspaper thinking about making a survival game](/cat.jpeg)

After much toil, I finally got my Enshrouded dedicated server running. If
there's an easier way, I would love to know it. Until then, here's what I did.

Like most projects of late, I started with making a Linux container via Proxmox.
All was well, until I learned that there is no Linux version of the server.
Looking at some docker images, it seems others have used Steam Deck methods for
running Windows programs in Linux, i.e. Proton or Wine or whatever.

Rather than adding another layer to the pile (Proxmox > Linux Container >
Docker Container), I thought I'd try my hand at making a Windows VM in Proxmox.
This was a journey in itself, having only had Linux-based virtual hosts in this
current Proxmox project. I remember setting up Windows VMs in Azure was quick
and painless, but hoping for as much in Proxmox felt like it'd be a stretch.
I tracked down a Windows 10 ISO, made it through installation, ran short on
disk space, added a 10GB volume. It was no where near as convenient as an LXC,
but after a couple hours, it was up and running.

Sadly, being Windows 10 Home edition, there was no remote desktop server feature
available. On second thought, Proxmox is somehow able to remote desktop into it;
I'll have to check this out later.

Then began the onerous arduous task of setting up the dedicated server. I've
been using Warp Terminal for many tasks, and find it the most useful AI dev
tool of the ones I've tried. Opening a port in the firewall, and installing steamcmd and server was
routine; I've done this a few times for other dedicated servers. The challenge
was transferring the save game from one of our Steam account's cloud storage to
run on the dedicated server.

I had tried this transfer before, at least a couple times, with no success. The
character would make it (their experience, inventory, uncovered map shroud),
but none of the craftspeople, structures, or world-things were there. I put Warp
to the task. One of the first obstacles, however, was that I could not figure
out how to allow public key SSH authentication for Windows machines.

Some info online recommends doing the usual, i.e. making `~/.ssh/authorized_keys`
if it doesn't exist, and simply adding your public key on an new line. This
had never worked for me and I had resigned myself to a lifetime of having to
type a password every friggin' time when ssh'ing into a Windows machine. But
then I got an idea: if this were a Linux box, I would check the `sshd_config` to
see whether public key auth was enabled. I figured Windows must have such a file
somewhere, and lo, it does!

Public key auth was commented out, with `yes` as the value, which I forget means
whether that is the default. But in this same file, it mentions another file,
`%PROGRAMDATA%/ssh/administrators_authorized_keys`, which at that time, did not
exist. I made the file and added my key to it... and it worked! I guess the
answer is that, if you are trying to connect as a user with admin privileges,
the public key must be in that admin key file. It felt good to finally crack
that, it was bothering me and now I am freed from the toil of entering one more
password.

With that out of the way, I could unleash Warp on the save game problem. I tried
the following:
* Transferring the most recent save files
* Transferring all the save files
* Transferring the most recent save files and removing the `-[number]` suffix
* Starting a new game in the server, building a thing or two, examine the files created by the server for clues

With every attempt, on startup the server would log something like:
```
0 structures 0 entities
```
And I would smack my forehead.

I tried many variations on the above, each with no success. I was burning through
my Warp monthly credits with no light at the end of the tunnel. I was skeptical
that simply renaming files would fix anything, it felt like a desperate, ignorant
move. The file names are mysterious though, just seemingly random alphanumeric strings.

Warp noticed something about the filenames: something about correlating to IDs,
though I am not sure which, maybe the server? It recommended trying getting the
most recent save files from the Steam user cloud storage, copying them to the
dedicated server (I had turned on Windows network sharing and mounted the VMs
directory with the server onto my bare metal machine as a network drive, this
made moving files around much easier. Warp was struggling with names with spaces,
and trashing between `cmd.exe` and Powershell), and then renaming them to use the server's ID (not sure what
that means), but then, miracle of miracles, it loaded structures and entities.
I connected as a client and beheld the splendor.