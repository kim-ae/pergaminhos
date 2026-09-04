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
15. n8n
16. Jellyfin
17. Tailscale
18. Proxmox
	1. Bind9
	2. Finish bind9 configuration
	3. PiHole
	4. nextclaude
19.  K8S kgateway as Reverse proxy for internal network, for DNS that needs certificate for HTTPS communication
20. Use truenas for PV (NFS)
	1. sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
21. Check issues with tailscale and cillium: https://tailscale.com/docs/features/kubernetes-operator#cilium-in-kube-proxy-replacement-mode
## Next steps
* should I use dynamic mapping for DNS with cloud flare? 
* Check openship tool
* Install some database on proxmox to have it managed in my infrastructure
* Install process exporter to have metrics per process on my linux machines
* Install NOMAD
* Transform everything in infrastructure as code by using tooling like ansible/terraform/puppet
* Migrate grafana to use datasource instead of filesystem for state.
* AnyType
	* I do want to improve my knowledge links page, I could use a database to be able to add through admin panel more information instead placing it in the code and having to deploy a new thing. Might be that I could do it by just suing anytype (?)
* Object Storage
* PfSense - firewall
* Check https://about.gitea.com/