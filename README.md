# DGX-Demo
內容包含了DGX Station A100基本的Demo場景  

### NVIDIA Multi-Instance 快速部屬工具(MIG-Parted)  
  NVIDIA開發的套件[MIG-Parted](https://github.com/NVIDIA/mig-parted "link")   
  透過yaml來快速將配置MIG，其中包含開啟MIG Mode 以及關閉 MIG Mode  
  
### NVIDIA GPU Cloud(NGC)   
  [NVIDIA GPU Cloud](https://ngc.nvidia.com/ "link")   
  [NGC Prviate Regustry User Guide](https://docs.nvidia.com/ngc/ngc-private-registry-user-guide/ "link")   
  DGX包含了NGC的private container images repositories，透過NGC來達到異地儲存images  
  
### NVIDIA System Management(NVSM)  
  [NVIDIA System Management User Guide](https://docs.nvidia.com/datacenter/nvsm/nvsm-user-guide/index.html "link")  
  NVSM為NVIDIA針對GPU管理的相關套件(DGX only)，藉由此套件來達到達到檢查目前DGX的硬體狀態  
  
### Monitoring  
  [NVIDIA System Management User Guide](https://github.com/NVIDIA/gpu-monitoring-tools "link")   
  透過包括DCGM-export, Prometheus, Grafana套件來達到GPU節點的監控(內文為Docker version)
