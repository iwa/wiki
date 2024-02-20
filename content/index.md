---
title: Welcome to my Homelab
---

<img align='right' src="https://avatars2.githubusercontent.com/u/19956672?s=460&u=a139c3a0107aef1cc9cf204eb1d10ad6e1737efc" width="230" style="max-width: 30%">

# Hi 👋 I'm iwa

I'm a French Apprentice Engineer in Software Dev based in Paris.
I like building my own things, thus why I went into coding.
But progressively, as I wanted to deploy my apps to be publicly accessible, I discovered the great world of Linux, Servers, and **Self-Hosting**!

After almost 5 years into hosting things, I'm writing this wiki as a kind of way to provide documentation on my homelab setup, while trying to explain things as clear as possible, to help anyone who would want to replicate some parts of my homelab.

Why I'm self-hosting?
- **Privacy**, it's a huge benefit for me to own my own data.
- **Learning**, the journey of self-hosting learn you a lot about how you manage servers, networking, and how you build an architecture of services.
- **Money**, self-hosting services you use daily can definitely help you get rid of those monthly subscriptions.
- **It's fun!** I really like coding, but I happened to LOVE managing my homelab like a mini datacenter.

If you wanna get into the hobby of self-hosting, I really recommend you to check out [Christian Lempa videos](https://www.youtube.com/@christianlempa), as well as reading [r/selfhosted](https://www.reddit.com/r/selfhosted/). These are really great ressources which can greatly help getting into *homelab-ing*.

## Homelab

My Homelab consists of 2 machines:

- **a Minisforum Mini-PC**
  - runs Proxmox
    - VM with Debian 12
  - runs all my self-hosted apps
- **a Hetzner arm64 VPS**
  - runs Debian 12
  - runs a [[reverse-proxy|Reverse Proxy]] and a [[Monitoring system]]
  - this VPS acts like my public gateway

I heavily rely on 2 core technologies:

- **[[docker|Docker]]**, all my apps run into containers
- **Tailscale**, plug-&-play Wireguard VPN integration
