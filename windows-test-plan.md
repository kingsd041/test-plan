# Windows Test Plan
 
 
 
## Testing Environments
- Linux image  
	kingsd/nginx:install-tools
- Windows image  
	mcr.microsoft.com/windows/servercore/iis:windowsservercore-ltsc2019
 
### Windows Node Version
- [x] Windows Service Core 1809
 
### VMs
- [ ] Azure
- [ ] Google Cloud
- [x] AWS
 
### CNI Support
- [x] Flannel (host-gw)
 
 
## Testing Cases
Refer: https://kubernetes.io/docs/getting-started-guides/windows/#supported-features
 
### Kubernetes Versions
- [x] 1.14.1
 
### Workloads
- [ ] Deployment
	- [x] Namespace
	- Port Mapping
		- [x] HostPort
		- [x] ClusterIP
	- [x] Environment Variables
	- [x] Node Scheduling
	- [x] Health Check
	- [ ] Volume
		- [x] empty, hostPath
		- [ ] pvc
	- [x] Scaing/Upgrade Policy
	- [x] Labels & Annotations
	- [x] Increase/decrease the scale of workloads
	- [x] Upgrade workloads
	- [x] Delete workloads
	- [x] Exec to pods
	- [x] View logs for pods
	- [x] Delete pods
- [x] Statefulset
- [x] Job
- [x] CronJob
- [x] Deamonset
 
### Pod scheduling
- [x] by node selector
- [x] by affinity
- [x] by taints
 
### Networking
- [x] Pod2Pod between Linux Pods via IP
- [x] Pod2Pod between Linux Pods via DNS
- [x] Pod2Pod between Windows and Linux Pods via IP
- [x] Pod2Pod between Windows and Linux Pods via DNS   
> Windows pod 只能通过FQDN访问，不支持 ClusterFirstWithHostNet，参见：https://kubernetes.io/docs/setup/windows/intro-windows-in-kubernetes/#dns-limitations   

- [x] Pod2Pod between Windows Pods via IP
- [x] Pod2Pod between Windows Pods via DNS
- [x] Pod2Service between Liunx Pod and Windows Service via IP
- [x] Pod2Service between Liunx Pod and Windows Service via DNS
- [x] Pod2Service between Liunx Pod and Windows Service via FQDN
- [x] Pod2Service between Windows Pod and Linux Service via IP
- [x] Pod2Service between Windows Pod and Linux Service via DNS   
- [x] Pod2Service between Windows Pod and Linux Service via FQDN
- [x] Access NodePort Service via Linux Node & Ingress
> 已知问题，Local NodePort access from the node itself fails (works for other nodes or external clients)    
> 参考：https://kubernetes.io/docs/setup/windows/intro-windows-in-kubernetes/#networking-1   

- [x] Access NodePort Service via Windows Node
- [x] Access external IPs (i.e. outbound NAT) from Linux Pod
- [x] Access external IPs (i.e. outbound NAT) from Windows Pod   
> 只能通过curl不能使用ping，参考：https://kubernetes.io/docs/setup/windows/intro-windows-in-kubernetes/#networking-1   
- [x] Access external DNS (i.e. outbound NAT) from Linux Pod
- [x] Access external DNS (i.e. outbound NAT) from Windows Pod   
> 只能通过curl不能使用ping，参考：https://kubernetes.io/docs/setup/windows/intro-windows-in-kubernetes/#networking-1     
- [x] Access Pod via Node
- [x] Access Node via Pod

### Services
- [x] NodePort
- [x] ClusterIP
- [x] LoadBalancer
- [x] ExternalName
- [x] Headless services

### Volume
- [x] emptyDir
- [x] hostPath
- [ ] other storage class of Cloud Provider (Azure, GCP, AWS)
 
### ConfigMap & Secret
- [x] environment variables
- [x] volume usages
 
### Cluster Metrics
- [x] CPU
- [x] RAM
 
### Restart & Recreate
- [x] restart host, then testing networking, volume, …
- [ ] restart rancher-agent (powershell: stop-service rancher-agent), then testing networking, volume, …   
> 重启rancher-agent，将导致pod重新创建，https://github.com/rancher/rancher/issues/20026   

- [x] delete the node from UI, stop and clean rancher-agent on host (powershell: stop-service rancher-agent; sc.exe delete rancher-agent), then testing networking, volume, …
 
### Long Time Running Status
- [x] after 2 days

