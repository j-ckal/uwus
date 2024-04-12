+++
title = "Increasing your internet privacy without a VPN"
date = "2024-04-12T18:33:05+01:00"
slug = "dns-privacy"
+++

The subject of privacy, surveillance and online security is a *unsightly* can of worms. I wanted to put together a short guide on tackling one aspect - DNS. 

### What's DNS!

It stands for Domain Name System, and it is used to associate domain names (such as [google.com](https://google.com), or [uwus.org](https://uwus.org) 😏) with IP addresses that individually identify servers on the internet - try going to [216.58.206.67](http://216.58.206.67) or [172.67.205.111](http://172.67.205.111)!)

### Privacy?

According to Google, around [90%](https://9to5google.com/2023/08/16/chrome-https-first-mode/)) of web traffic nowadays uses the HTTPS protocol. This protocol ensures traffic between your browser and the website is verified and encrypted, meaning if anyone is snooping on your network all they'll see is gibberish. 

This is great, and it means attackers can't intercept sensitive information like card details when you're shopping online, as much as VPN providers would like to convince you otherwise.

*... Unless the website you're shopping on doesn't use HTTPS, in which case I'd close the tab. Secure transmission is a requirement in PCI DSS standards, which all reputable payment providers mandate.*

However! In order to navigate to a website, DNS is still required to resolve ASOS.com to 123.456.789, and anyone snooping will have a comprehensive log of the domains you're visiting. This can be used by bad guys, network administrators or advertisers to build a profile on the services you use.

On the good side, once the top-level domain has been resolved (if HTTPS is used) snoopers won't be able to tell which specific page of the website you're on (e.g. not *ASOS.com/cute-sexy-sunglasses*, only *ASOS.com*). This might be fine by you, and in that case you can stop here.

### What if it's not fine by me

Secure DNS is supported by most modern browsers and devices. Before making changes, I'd suggest you do some research on which DNS provider you'd like to go with. 

Personally I use [Quad9](https://www.quad9.net/), a non-profit provider [funded by IBM, PCH and GCA](https://www.quad9.net/news/blog/quad9-and-your-data/).
[Google](https://developers.google.com/speed/public-dns/docs/using) and [Cloudflare](https://one.one.one.one/) also offer private DNS services.

- If you're on Android, Private DNS should be an [option in settings](https://www.zdnet.com/article/how-to-turn-on-private-dns-mode-on-android-and-why-you-should/) that you can enable.
- on iOS and macOS, You can use a configuration file. [This GitHub repo](https://github.com/paulmillr/encrypted-dns) offers pre-made configurations for a range of providers and you can install from the list.
- Alternatively on iOS, you can install the [Cloudflare 1.1.1.1](https://one.one.one.one/) app - although this locks you to Cloudflare's DNS service.
- On modern versions of Windows, you can configure secure DNS in [network settings](https://learn.microsoft.com/en-us/windows-server/networking/dns/doh-client-support).
- If you're feeling fancy, you can set up a DNS server on your home network on a Raspberry Pi (or any home server) and configure your router to direct all DNS traffic through it. It's relatively straightforward if you're comfortable in the terminal, see [Pi-hole docs](https://docs.pi-hole.net/guides/dns/cloudflared/) for more info.

### Anything else?

Server name indication might still out you. SNI is the first step in a TLS handshake, the protocol used to secure your HTTPS traffic. Encrypted SNI is slowly becoming more common though and, if you want, you can read more about it on [Cloudflare's website](https://www.cloudflare.com/en-gb/learning/ssl/what-is-encrypted-sni/).

That's all for now :)