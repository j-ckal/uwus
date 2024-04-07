+++
title = 'My humble home-lab'
date = 2023-05-29
draft = false
+++

## Intro

I've been tinkering with secondary computers since I got a Pi 3B for my birthday in 2016, but didn't really use it for what I imagined a server to be, outside of installing Pi-hole to do DNS-level ad blocking. I wasn't that interested in the GPIO capabilities outside of a [university project](https://www.youtube.com/watch?v=J1y6sschMHc), and wanted something more powerful to play with.

![](/images/homelab/pc.jpg)

## Enter, the HP ProDesk!

![](/images/homelab/hp.jpg)

I found this back in February, and aside from looking like it was pulled from a school computer lab, it checked all my boxes:

- Power efficient - Reasonably modern T-sku CPU (Full support for [ACPI C-states](https://metebalci.com/blog/a-minimum-complete-tutorial-of-cpu-power-management-c-states-and-p-states/)), uses approx 5W while idle
- Hyperthreading for multithreaded workloads
- 8GB RAM + 256GB M.2 SSD included
- £70, which was cheaper than anything else I could find at the time

The only issue was 1 broken USB port which I didn't mind, and a blemish I couldn't find.

For now, I'm using an old USB [LaCie drive](https://www.apple.com/uk/shop/product/HKVD2ZM/A/lacie-1tb-rugged-usb-c-portable-drive) I had lying around for media storage, formatted in exFAT so I can still read/write from macOS & Windows.

It arrived running Windows 10 with a registered project key (!) alas, I formatted it and installed Ubuntu Server 22.04 as I'm most familar with it (linuxbros pls forgive me). I updated & upgraded, copied over my SSH keys, and was ready to roll!

## Containers
Previouslly I had Home Assistant and Plex ~~running~~ crawling on my Raspberry Pi, and this was my first experience managing multiple services running on bare metal on one machine. 

As you can imagine, dealing with conflicting ports and dependencies was a headache - [*looking at you, Pi-hole*](https://github.com/pi-hole/pi-hole/issues/1785) - and since I was starting from fresh I wanted to learn more about containerisation.

I migrated existing services over to the new containerised versions, which was surprisingly painless. I used docker-compose to deploy them, with much help from the [linuxserver.io images & docs](https://linuxserver.io), transferring my existing configs for each service.

I later learnt about [Portainer](https://portainer.io) which I now use to reduce the amount of time spent in the command line, since it allows for monitoring & spinning up/down containers in a web UI.

![](/images/homelab/portainer.jpg)

## External Access

For a while I exposed the *raw, unadulterated ports* of my self-hosted services to the internet, and watched fail2ban go to work on the deluge of bot attacks, predominately originating from China and DigitalOcean. This made me uneasy, and I wanted to add another layer of security to my home-lab.

![](/images/homelab/fail2banlogs.jpg)

I'm using a Cloudflare Tunnel to expose Home Assistant & SSH to the internet, protected behind SSO. It's impressive how many Cloudflare services are available on the free tier... This method mitigates some risks but introduces some new ones:

- No longer exposing any ports from my home network, preventing bots from bruteforcing/exploiting possible future vulnerabilities 👍
- An approved login method, verified through CloudFlare, is now required before any services can be reached 👍
- Since Cloudflare is proxying traffic going to/from my server, they now have the ability to inspect/analyse this 👎
- I now have a permanently running, self-updating binary running that has access to my system & local network (I could improve this with segmentation, but for now 👎)

I followed a few different [tutorials](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/use-cases/ssh/#connect-to-ssh-server-with-cloudflared-access) on how to tunnel different protocols and applications. The process basically involves:

1. Setting up DNS records on your domain to point towards Cloudflare
2. Downloading & configuring the [cloudflared](https://github.com/cloudflare/cloudflared) client with your token
3. Assigning it a place on your domain (e.g. *ssh.example.org*) 
4. And if you want to add IAM (which I'd highly recommend), setting it up as an as an [Application](https://developers.cloudflare.com/cloudflare-one/applications/configure-apps/self-hosted-apps/)

Now I can use Home Assistant or SSH into my machine from anywhere with a pretty URL and login screen! 😇

![](/images/homelab/cfzero.jpg)
