---
title: Jellyfin does not suck
description: Adventures in serving music
date: 2023-11-11 18:12:07 +0000
tags: music tech
---

I have spent the afternoon getting my mind around how to
make the music on my hard-drive available to the local
network.  (Yeah, I know this is all a bit old school but,
hey, I am 67 and I am allowed to be old school).

For many years, I used Logitech Media Server (LMS) for this
sort of thing because I had a Squeezebox Boom.  I loved that
device: it sat in a corner of the kitchen and played music
happily, controlled from my phone by the excellent [Orange
Squeeze](https://play.google.com/store/apps/details?id=com.orangebikelabs.orangesqueeze&hl=en&gl=US)
app.  Sadly, after getting through two new
power supplies, the Boom's tiny screen faded to invisibility
and the thing became unusable.  Nevertheless, I carried on
using LMS with a uPnP plugin to talk to a new, much cheaper,
speaker.  This sort of worked but not reliably.

All this came to a head on account me changing OS to an
arch-based distribution and so having to re-install all
infrastructure.  LMS is available and installs OK but I
could not get it to talk to my phone or the speaker.  Time
for a rethink: since the Boom doesn't work anymore, there is
no need to use LMS so are there any alternatives?

The interwebs threw up [Jellyfin](https://jellyfin.org) as a
possibility so I gave it a spin and was very
impressed!

* It installs easily via `pacman` (you need to get both
`jellyfin-server` and `jellyfin-web`).  Enable and start the
service from `systemctl` and you are (almost) good to go.
* Now head to `localhost:8096` and discover a clean web
  interface to help you set things up.
* I then spent ages trying to work out why `jellyfin` was
  not visible to the rest of the network.  Eventually, the
  penny dropped that you have to open some ports on the
  firewall (doh!).
* At this point, my phone sees jellyfin as does the uPnP
  speaker and everything is golden.
  
Why do I like it so much?

1. It works.
2. The web interface is really pretty.  It does the "Wall of
   Album Covers" thing very well:
   ![Screenshot showing wall of album covers](/assets/img/screenshot.png)
3. It gets its metadata from
   [musicbrainz](https://musicbrainz.org/).
4. It also serves videos and photos.
5. It is free as in both freedom and beer and looks like it
   will stay that way.
5. Did I say It Works?  It really does.
