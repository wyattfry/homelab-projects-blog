---
date: '2025-05-02T10:27:15-04:00'
draft: false
title: 'Network Attached Storage'
---

I realized that, because a Proxmox node is doing double-duty as a NFS server, and all three nodes use said server for their disks, if node one goes down, everyone does.

In view of this, I thought it'd be a good move to have a dedicated storage machine. I considered a few options, but decided I'd like to build my own machine. Whether it will end up costing less than an off-the-shelf NAS box, we'll see. But I think it will at least be comparable, and a lot more fun. Excuse me, "fun".

I started sketching it out, my requirements were as follows:

* Fit in a 3U space
* Use as much on-hand (i.e. paid for) material as possible
* At least 2 HDDs for storage (OS can go on whatever)
* Have expansion potential (more disks, more nodes)

![alt](/NAS.svg)

## Parts List

| Component                     | Cost    | Status  |
|------------------------------|---------|---------|
| Motherboard A78M-E35 with CPU| $42.63  | ordered |
| 16GB RAM DDR3                | $19.73  | secured |
| AIO CPU Cooler               | $31.98  | ordered |
| PSU                          | $0.00   | secured |
| 2TB HDD (from pve-01)        | $0.00   | secured |
| 2TB HDD                      | $31.98  | ordered |
| 500GB HDD                    | $7.00   | secured |
| 128GB SSD                    | $21.31  | ordered |
| Case fans                    | $20.25  | ordered |
| Screws                       | $18.99  | ordered |
| Servos for gauges            | $23.36  | ordered |
| CPU power extension cord     | $10.95  | ordered |
| Arduino                      | $0.00   | secured |
| **Total**                    | **$228.18** |         |

## Fun Touch

I thought it'd be fun to have "empty-full' analog gauges on the front, so I'll try it out with an Arduino and some servos. See the library project for more on things like this.