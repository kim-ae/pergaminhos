Goal: get a Tailscale peer connection off DERP relay and onto a direct WireGuard path, or reduce the cost of DERP when direct is impossible. Builds on the NAT traversal theory in [[Tailscale Internals]].

## Step 1 - Confirm you are actually on DERP

Use `tailscale status` / `tailscale ping` (see `0Tips.md` in this Section for the exact commands and sample output) to confirm the peer is on `relay derp-<region>` rather than `direct <ip:port>` before spending effort optimizing. From the phone, the Tailscale app's per-node connection info shows the same thing as **"Direct"** or **"via DERP (region)"**.

## Option 1 - Port forward UDP 41641 (most effective)

If one side has a static, reachable endpoint, hole punching is unnecessary. On the home router:
```
UDP 41641 -> 192.168.0.101:41641   (the controlplane node)
```
This gives the remote peer (e.g. a phone on carrier data) a stable endpoint to dial directly, without needing the simultaneous-send hole-punch trick from [[Tailscale Internals]] to succeed.

## Option 2 - Check the carrier NAT type

Symmetric NAT (common on mobile data networks) makes hole punching impossible **regardless** of home router configuration, because the carrier itself assigns a different external port per destination. If that is the case, DERP is unavoidable unless a VPS is introduced (Option 3).
```bash
tailscale debug derp-map   # shows which DERP regions are in play
```

## Option 3 - Self-host a DERP relay (lower latency, still not direct)

Useful when symmetric NAT is confirmed unavoidable and the goal shifts from "go direct" to "keep the relay traffic on infrastructure I control."

Run the relay on a VPS with a public IP:
```bash
docker run tailscale/derper -hostname derp.yourdomain.com -certmode manual
```
Then point the tailnet's ACL policy at it:
```json
"derpMap": {
  "Regions": {
    "900": {
      "RegionID": 900,
      "RegionCode": "custom",
      "Nodes": [{ "Name": "1", "RegionID": 900, "HostName": "derp.yourdomain.com" }]
    }
  }
}
```
Traffic is still relayed - just on infrastructure you own instead of Tailscale's public DERP fleet. This mainly buys latency control and data-residency, not a direct connection.

#tailscale #derp #nat-traversal #self-hosted
