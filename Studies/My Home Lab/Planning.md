## What we have
1. kubeadmin
2. Node configuration
3. Cillium
4. Cillium gateway
5. Flux
6. Kubeseal
7. Grafana Alloy
8. Remove grafana alloy for metrics
9. Prometheus
10. MetalLB
11. Remove Cillium gateway
12. Install kgateway
13. Grafana
14. Loki
15. Bind9
16. n8n
17. Jellyfin
18. Tailscale
19. Finish bind9 configuration
20. Proxmox
## Next steps
* TrueNAS - only for NAS
	* sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
* Use truenas for PV
* Reverse proxy ( nginx? ) - should I use dynamic mapping for DNS with cloud flare? - NO I will use k8s as my entry point gateway, either by providing entrypoint from external traffic or internal traffic depending on the application.
* AnyType
	* I do want to improve my knowledge links page, I could use a database to be able to add through admin panel more information instead placing it in the code and having to deploy a new thing. Might be that I could do it by just suing anytype (?)
* PiHole
* Object Storage
* PfSense - firewall
* Install local provisioner https://github.com/kubernetes-sigs/sig-storage-local-static-provisioner
* Check issues with tailscale and cillium: https://tailscale.com/docs/features/kubernetes-operator#cilium-in-kube-proxy-replacement-mode