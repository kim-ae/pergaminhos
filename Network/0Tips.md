## Check Tailscale connection path (direct vs DERP)
#tailscale #cli #diagnostics
```
tailscale status
# 100.64.0.5   your-phone   direct  203.0.113.5:54321   <- direct WireGuard
# 100.64.0.5   your-phone   relay   derp-sao             <- going through DERP
```

## Ping a peer and see the actual path used
#tailscale #cli #diagnostics
```
tailscale ping 100.64.0.5   # peer's Tailscale IP

# Direct:
# pong from your-phone (100.64.0.5) via 203.0.113.5:54321 in 12ms
# DERP:
# pong from your-phone (100.64.0.5) via DERP(sao) in 45ms
```

## See which DERP regions are in use
#tailscale #cli #derp
```
tailscale debug derp-map
```

## Check the local ARP cache
#arp #cli #diagnostics
```
arp -n
# Address         HWtype  HWaddress           Flags
# 192.168.0.1     ether   d8:0d:17:aa:bb:cc   C
# 192.168.10.2    ether   (incomplete)         -   <- unresolved, traffic won't reach it
```

## Test whether an IP resolves via ARP on the local segment
#arp #cli #diagnostics #metallb #cilium
```
arping 192.168.10.2
# success from LAN confirms Cilium L2 Announcements / MetalLB is
# answering ARP for that VIP correctly on this segment
```
