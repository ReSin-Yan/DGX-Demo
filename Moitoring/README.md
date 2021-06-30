[DCGM-export](https://github.com/NVIDIA/gpu-monitoring-tools "link")  
[Prometheus](https://prometheus.io/docs/prometheus/latest/installation/ "link")  
[Grafana](https://grafana.com/docs/grafana/latest/installation/docker/ "link")  
# 安裝
### DCGM-export
安裝DCGM-export的套件，需要確定DCGM服務是開啟的
sudo docker run -d --gpus all --rm -p 9400:9400 nvcr.io/nvidia/k8s/dcgm-exporter:2.0.13-2.1.2-ubuntu18.04
確認服務是否起來  
curl [DGX-Station IP]:9400/metrics
這邊額外建議除了監控GPU以外，另外安裝Prometheus的node監控  
可以看到CPU、Memory、I/O之類的資訊  
### 安裝prometheus
sudo mkdir -p /etc/prometheus
cd prometheus/
需要寫入一份設定檔案  
預設的設定檔按如
#https://github.com/prometheus/prometheus/blob/39d79c3cfb86c47d6bc06a9e9317af582f1833bb/documentation/examples/prometheus.yml
或是可以使用此資料夾內的檔案

修改IP


之後建立服務
sudo docker run -p 9090:9090 -d -v /home/nvidia/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus 

#check with browser
IP

之後可以在這邊搜尋到
圖片圖片

### 安裝 grafana
sudo docker run -d -p 3000:3000 grafana/grafana
#check with browser
http://192.168.0.127:3000/
admin/admin
#set data source
