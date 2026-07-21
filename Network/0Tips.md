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
