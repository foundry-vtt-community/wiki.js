---
title: Netbird Mesh
description: Only use if you and everyone in your group are willing to install NetBird to access your self-hosted Foundry instance
published: false
date: 2026-05-07T19:55:47.065Z
tags: hosting, self-hosting, netbird, cgnat, vpn, wireguard
editor: markdown
dateCreated: 2026-05-07T19:55:47.065Z
---

# NetBird Mesh Setup

NetBird is a mesh VPN built on WireGuard. The company is headquartered in Berlin and the software is open source.

This guide uses NetBird's free hosted service to give your group private access to Foundry. Everyone installs NetBird, joins your network, and connects to the host computer's NetBird address. There is no port forwarding and no public exposure.

The trade-off is that everyone has to install NetBird before they can play. If your group will not install software, see [Tailscale Funnel](/en/setup/hosting/tailscale) or [Cloudflare Proxying & Tunnel](/en/setup/hosting/cloudflare-proxy-tunnel).

NetBird's free tier covers up to 100 devices and 5 users. Clients run on Windows, macOS, Linux, iOS, and Android.

## Setup

### Host

Install NetBird on the computer running Foundry from <https://docs.netbird.io/how-to/installation>. Run `netbird up` (or use the GUI on Windows and macOS) and authenticate.

Open <https://app.netbird.io/peers>. The host will appear in the peer list. Note its NetBird IP — it looks like `100.x.x.x`.

### Adding your group

Invite each person by email from the **Users** page in the dashboard, or generate a setup key from the **Setup Keys** page and share it.

Email invites are simpler — each person creates their own NetBird account and joins your network automatically. Setup keys skip the account creation but require sharing the key.

### Each person in the group

Install NetBird from <https://docs.netbird.io/how-to/installation>, then sign in or paste the setup key.

Open `http://<host-netbird-ip>:30000` in a browser, replacing `<host-netbird-ip>` with the IP from the host setup.

If you want a friendlier address than `100.x.x.x`, rename the host in the dashboard. NetBird assigns it a hostname under your network domain like `foundry.netbird.cloud`.

## Locking down the network

By default, anyone on your NetBird network can reach the host. Set up an ACL on the **Access Control** page to restrict the group to only port 30000.

Set a password on the Gamemaster account in any Foundry world.

## Privacy

NetBird's hosted control plane sees connection metadata — which devices are connected, when, and their public IPs. It cannot decrypt your Foundry traffic, which is encrypted end-to-end by WireGuard between devices.

## Troubleshooting

**Someone can't connect.** Check <https://app.netbird.io/peers>. If they show as offline, their NetBird client isn't running. If they show as connected but can't reach Foundry, the host's firewall is blocking port 30000 — check `ufw`, `firewalld`, or `iptables` on Linux, or Windows Defender Firewall on Windows.

**Someone reaches the host but Foundry doesn't load.** Foundry isn't running on the host. Check `http://localhost:30000` from the host itself.

**ACLs are blocking someone.** Confirm they're included in the policy that allows port 30000 to the host.