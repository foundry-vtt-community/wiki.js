---
title: Netbird Mesh
description: Only use if you and everyone in your group are willing to install NetBird to access your self-hosted Foundry instance
published: true
date: 2026-05-07T20:25:55.940Z
tags: hosting, self-hosting, netbird, cgnat, vpn, wireguard
editor: markdown
dateCreated: 2026-05-07T19:55:47.065Z
---

# NetBird Mesh Setup

NetBird is a mesh VPN built on WireGuard. It's open source and the company is based in Berlin, which is part of the appeal if you'd rather not route your gaming traffic through a US-based service.

This guide gets your group onto a private NetBird network so they can connect to your Foundry server without you having to port forward, expose anything publicly, or trust a third-party tunnel like Cloudflare. The catch is that everyone needs to install NetBird first. If your group is allergic to installing software, [Tailscale Funnel](/en/setup/hosting/tailscale) is the easier path — public URL, no install needed, but you're trusting a closed-source service for it.

The free hosted tier from NetBird covers up to 100 devices and 5 users, which is plenty for any home Foundry setup. Clients exist for Windows, macOS, Linux, iOS, and Android, so nobody is going to be left out.

## Setting up the host

First, install NetBird on the computer running Foundry. The official install instructions for every supported OS are at <https://docs.netbird.io/get-started/install> — pick yours and follow along. Once it's installed, run `netbird up` (or use the system tray app on Windows and macOS) and authenticate with your NetBird account.

Now log into the NetBird dashboard at <https://netbird.io> and head to the **Peers** section. Your host should show up in the peer list within a few seconds. Take note of its NetBird IP — it'll be a `100.x.x.x` address. That's the address everyone will use to reach Foundry.

If `100.91.42.17` is ugly to you (and it is, let's be honest), you can rename the host in the dashboard and NetBird will give it a real hostname like `foundry.netbird.cloud` instead. Worth doing.

## Adding your group

There are two ways to get your group onto the network: invite them by email, or hand out a setup key.

The email route is the easier one for non-technical players. Go to **Team → Users** in the dashboard and click **Invite User** for each person. They'll get a link, sign up for their own NetBird account, and join your network automatically.

The setup key route skips the account creation step — go to **Setup Keys** in the dashboard, generate a key, and share it through whatever chat you use. Faster, but anyone with the key can join, so don't paste it into your campaign's public Discord.

## What each person in the group does

Each person installs NetBird from <https://docs.netbird.io/get-started/install>, signs in (or pastes the setup key), and then opens `http://<host-ip>:30000` in their browser, where `<host-ip>` is the address you noted earlier.

That's it for the connection part. From here it should look exactly like a normal Foundry session.

## Locking it down

By default, NetBird creates a `Default` policy that allows every peer on your network to talk to every other peer on every port. That's fine for a small private network where you trust everyone, but if you want to be careful — say, prevent a compromised group member's machine from poking at your host on anything other than Foundry — you can replace it with a tighter policy.

The flow is roughly:

1. Go to **Access Control → Groups** and create two groups: one for the host (e.g. `foundry-host`) and one for your group (e.g. `players`). Assign each peer to the right group from the **Peers** page.
2. Go to **Access Control → Policies** and click **Add policy**. Set source to `players`, destination to `foundry-host`, protocol to TCP, port to 30000.
3. Once the new policy is in place, delete the `Default` policy. Without that step, the default rules still apply and the new restriction does nothing.

Set a password on the GM account in your Foundry world either way. Sounds obvious, but a lot of people skip it because it's a private network. It's the last line of defense if anyone unexpected gets onto your network somehow.

## On privacy

The hosted version of NetBird means NetBird's company can see metadata about your network — which devices are connected, when, and what their public IPs are. They cannot see your Foundry traffic itself, because WireGuard handles end-to-end encryption between your devices and the private keys never touch their servers. But the metadata visibility is there.

If that bothers you, the alternative is self-hosting NetBird's control plane on your own VPS, which removes their visibility entirely. That's a bigger project than this guide covers, but the official docs at <https://docs.netbird.io/selfhosted/selfhosted-quickstart> are a starting point.

## Things that go wrong

**Someone can't connect at all.** Check the **Peers** page in the dashboard. If their device shows offline, the NetBird client isn't running on their end — tell them to start it.

**They show as connected but can't reach Foundry.** The host's firewall is blocking port 30000 from the NetBird interface. On Linux check `ufw`, `firewalld`, or `iptables` — the NetBird interface is `wt0`. On Windows check Windows Defender Firewall.

**The host is up but Foundry doesn't load.** Foundry isn't actually running on the host, or it's listening on a different port. From the host machine itself, check `http://localhost:30000` to confirm Foundry is up.

**You set up an ACL and now nothing works.** Either the new policy doesn't include the right peer or port, or you forgot to delete the `Default` policy after creating the new one. Check **Access Control → Policies** and make sure the right rule is in place and the Default is gone.