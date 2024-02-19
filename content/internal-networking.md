---
title: Internal Networking with URLs and HTTPS
---

In order to use vanity URLs and HTTPS (e.g., `https://service.iwa.sh`) and avoid having to use `http://ip:port` every time I want to connect to a service, I had to put in place a pretty complex networking system.

It's pretty dense but totally worth it, in my opinion. Let me guide you on how to set up a system like mine.

> **Services I use:**
> - Tailscale
> - Traefik
> - NextDNS
> - Cloudflare DNS
> - A domain name

> [!warning] Disclaimer
> If you want to publish and expose your services to the Internet, this is not a guide to follow, as here we focus on private and non-published services.

## Step 0. Use a VPN

It's totally optional, but in my case, I solely rely on Tailscale when accessing my services. Even when I'm at home, I still use Tailscale. As it automatically routes to a direct connection on the local network if possible, I don't feel the need to switch between them. Moreover, it would be a mess to switch from Tailscale IPs to local IPs every time. Yes, using local IP is more efficient, but the difference in bandwidth is really marginal nowadays.

## Step 1. Reverse Proxy
### 1.1 Setup your Reverse Proxy

I have set up a second instance of [[reverse-proxy.md|traefik]] on my main server that is not published and is only used as a local reverse proxy. This way, I have a reverse proxy whose only purpose is to serve my private services.

You have to expose ports `80/tcp` and `443/tcp`, but **DON'T** open them on your internet router, as that would totally go against what we're building.

### 1.2 Setup Automatic SSL

On pretty much all reverse proxies nowadays, you can configure automatic SSL certificate acquisition. They will most likely use Let's Encrypt to generate those certificates, which is totally fine, except for one detail.

By default, Let's Encrypt relies on the HTTP challenge to prove that the domain you're requesting a certificate for really belongs to you. But, as we do not want to expose our reverse proxy, we have to find another way to prove our ownership.

Luckily, if you use Cloudflare as your DNS, you can set up an API key that will prove your ownership through the [DNS challenge](https://doc.traefik.io/traefik/https/acme/#providers).

In traefik, you can configure a certificate resolver like so:

```yaml
certificatesResolvers:
  cloudflare:
    acme:
      email: your-email@example.com
      storage: /letsencrypt/acme.json
      dnsChallenge:
        provider: cloudflare
```

Then, you would need to set up an API key. To determine which type, you can follow the [official docs](https://go-acme.github.io/lego/dns/cloudflare/).

For a modern approach, you have the choice to set the environment variable either to `CF_DNS_API_TOKEN` or `CF_ZONE_API_TOKEN`, with `DNS:Edit` permission and `Zone:Read` permission, respectively.

### 1.3 Connect your apps

With [[reverse-proxy.md|traefik]], I'll add labels to all the services I want to access through my private reverse proxy. In their compose files, I'll then add:

```yaml
...
  labels:
    - "traefik.enable=true"
    - "traefik.http.routers.whoami.rule=Host(`whoami.server.example.com`)"
    - "traefik.http.routers.whoami.entrypoints=websecure"
    - "traefik.http.routers.whoami.tls.certresolver=myresolver"
    - "traefik.http.services.whoami.loadbalancer.server.port=80"

networks:
  default:
    name: traefik_link
    external: true
```

## Step 2. Custom DNS

In order to use our reverse proxy, we need a way to indicate to our device that those URLs need to be forwarded to your server. We do so by using plain DNS.

The thing is, I did not want to expose what my Tailscale IPs were. Even if it's riskless as long as my account doesn't get hacked, I was not comfortable with that.

As I was already using NextDNS for their ad-blocking features, the choice seemed obvious.

In NextDNS (as well as Pi-Hole and Adguard Home), you can set up custom rewrites. We can take advantage of that by using them to expose to ourselves the DNS record pointing to our server.

![[assets/nextdns-rewrite.png]]

Now, the domain `server.example.com` and all its subdomains would lead to your server.

## Step 3. Configure your devices

The last step is to configure your devices to use your custom DNS server instead of the default ones.

The way I'm doing it is by again using Tailscale. In Tailscale, you can enforce DNS settings to your devices that will apply as long as they're connected to your tailnet.

![[assets/tailscale-dns-setting.png]]

Be sure to check the `Override local DNS` option.

## Step 4. Enjoy!