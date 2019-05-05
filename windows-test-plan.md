# Windows Test Plan
 
 
 
## Testing Environments
- Linux image  
	kingsd/nginx:install-tools
- Windows image  
	mcr.microsoft.com/windows/servercore/iis:windowsservercore-ltsc2019
 
### Windows Node Version
- [ ] Windows Service Core 1809
 
### VMs
- [ ] Azure
- [ ] Google Cloud
- [ ] AWS
 
### CNI Support
- [ ] Flannel (host-gw)
 
 
## Testing Cases
Refer: https://kubernetes.io/docs/getting-started-guides/windows/#supported-features
 
### Kubernetes Versions
- [ ] 1.14.1
 
### Workloads
- [ ] Deployment
	- Namespace
	- Port Mapping
		- HostPort
		- ClusterIP
	- Environment Variables
	- Node Scheduling
	- Health Check
	- Volume
		- empty, hostPath
		- pvc
	- Scaing/Upgrade Policy
	- Labels & Annotations
	- Increase/decrease the scale of workloads
	- Upgrade workloads
	- Delete workloads
	- Exec to pods
	- View logs for pods
	- Delete pods
- [ ] Statefulset
- [ ] Job
- [ ] CronJob
- [ ] Deamonset
 
### Pod scheduling
- [ ] by node selector
- [ ] by affinity
- [ ] by taints
 
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
- [ ] Pod2Service between Windows Pod and Linux Service via DNS   
> 已知问题，参见：https://kubernetes.io/docs/setup/windows/intro-windows-in-kubernetes/#dns-limitations 
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
- [ ] NodePort
- [ ] ClusterIP
- [ ] LoadBalancer
- [ ] ExternalName
- [ ] Headless services

### Volume
- [ ] emptyDir
- [ ] hostPath
- [ ] other storage class of Cloud Provider (Azure, GCP, AWS)
 
### ConfigMap & Secret
- [ ] environment variables
- [ ] volume usages
 
### Cluster Metrics
- [ ] CPU
- [ ] RAM
- [ ] Disk
 
### Restart & Recreate
- [ ] restart host, then testing networking, volume, …
- [ ] restart rancher-agent (powershell: stop-service rancher-agent), then testing networking, volume, …
- [ ] delete the node from UI, stop and clean rancher-agent on host (powershell: stop-service rancher-agent; sc.exe delete rancher-agent), then testing networking, volume, …
 
### Long Time Running Status
- [ ] after 2 hours
