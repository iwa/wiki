---
title: Docker
---

Except for Tailscale, which runs bare metal, all of my applications run through containers. It's a choice of convenience and security. I didn't want to have to manually install all of my apps and risk one breaking the entire OS.

Docker has proven to be the best solution for my concerns. It runs containers (LXC) in which the apps are operating. Each app runs in its own container, providing a great layer of security.

> [!question] How containers are different to VMs?
> The difference lies in the management of the kernel. In short, the kernel is the program that interfaces with the hardware and the software. <br/>
> When running a VM, it runs its own kernel inside it, which can be quite heavy for the machine.<br/>
> On the other hand, containers share the same kernel as the host machine. It's worth noting that containers are a __Linux technology__; although it's possible to run them on macOS or Windows, it involves some sort of virtualization of Linux under the hood.

## How I manage my containers

[Portainer](https://docs.portainer.io/start/install-ce).

It's the best UI you could ask for managing your Docker environment.

It has everything you need - you can manage your containers, compose stacks, networks, volumes, and images.<br/>
Additionally, it offers an API, so you don't need to expose your Docker socket and risk associated security issues.

Using Portainer, I put all my apps into stacks in order to isolate them by group of apps. For example, I have a stack only for Vaultwarden, my password manager. Another one only for Traefik. Another one for my private Minecraft server. And the list goes on...

Doing so helps a lot keeping your architecture clean and manageable, you can edit your stack by changing 2 lines and it'll redoploy everything in the right order with the right connections to networks and volumes.

You have to get confortable using Docker Compose, although it's really not that difficult when you have a little experience in Docker.