---
date: '2025-05-02T10:27:20-04:00'
draft: false
title: 'Bears: The Making of a Game'
cover: 
  image: /bears.png
---

### Making a multiplayer browser-based game based on the card game "Bear Valley"

[Bears Game](https://bears.wyattfry.com)

![alt](/bears.png)

It started when I found a little card game at the library's used game sale a
few months ago. I played it a couple times with my son and thought it was pretty
fun. I've been on the lookout for a new software project, and turning a physical
game into a code was an appealing choice because you already have something to
set as a goal. It's like drawing for me; I'll think I want to draw, sit down
with a blank page and wonder what to draw. I usually end up drawing whatever
is around me.

I've been using generative AI for a lot of the project, then intervening if it's
stuck in a loop, or I'm not getting my point across. My personal goals for the
project were as follows:

* Use Go more
* Set up my own infrastructure
* Collaborate
* Have fun with friends

It's been slow going. I'm already a month in and haven't gotten to the game
proper yet. I intend to keep a running record on this blog of the development.
Here's a summary of the project so far:

* Tool selection
    * Backend in Go with Gorilla for websockets / realtime
    * Gow for Go development watcher
    * Frontend in Svelte / Sveltekit / Vite
    * Svelete Static Site Adapter
    * Generative AI
        * Warp Terminal
        * Cursor IDE
        * ChatGPT
* Infrastructure
    * Cloudflare DNS with SSL Termination
    * Router Port Forwarding to Caddy Reverse Proxy (Proxmox / Debian LXC)
    * Game Server (Proxmox / Debian LXC)
        * Running a GitHub Action Runner that builds frontend to static assets
        * systemd Service managing lifecycle of game service
    * No persistence yet, all in-memory

I was debating how to organize the server. I'm accustomed to a service oriented
architecture, a.k.a. microservices, i.e. running front and backends separately.
However, given that this is just an exercise and not needing to scale, I decided
to keep it simple and not use dynamic routing in the frontend so it could
compile to static assets, then serve the frontend and API (REST and websocket)
from a single process. For development I'll run them separately, but deploying
everything as one program solves some problems, not having to deal with CORS,
or with addressing. It did require some refactoring of the backend, making it
able to serve all three purposes, but it was fairly minor changes.

At the moment, the only parts implemented are the following:

1. Title Page: just has a start button
2. Enter Player Name: creates a player or if returning, allows editing your name
3. List Games: allows players to either join a game or create a new one
4. Create Game: set some values like name, mode, map
5. Game Lobby: players choose their 'character' and once all are ready, the game
proper may begin

It's hard to not get distracted and want to start polishing or adding
non-critical-path features to the exisiting flow. I'm dying to actually start
on the game itself, and I have started to sketch out the foundations for it.
The backend has the beginnings of a configuration system and a rules engine that
will declaratively define the "game" itself.

I'm also scratching my head a little on the frontend design for the game: the
card game its based on arranges standard rectangular cards in a hexagonal pattern,
so I figured I would opt for plain-old-hexagons. However, frontend is not my
strength, so I'm curious how to approach this one. I have made a few little games
with HTML canvas, but I'm not sure if that's necessary here, or if I can just
use Svelte.

I know I could use Kubernetes for all this, but having missed that era in the
industry, I'm also using these projects to experience the days before container
orchestration and feel some of the pain of wrangling virtual hosts. Though I am
really digging Linux Containers, but that's a topic for another post.
