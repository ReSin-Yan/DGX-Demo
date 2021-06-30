[DCGM-export](https://github.com/NVIDIA/gpu-monitoring-tools "link")  
[Prometheus](https://prometheus.io/docs/prometheus/latest/installation/ "link")  
[Grafana](https://grafana.com/docs/grafana/latest/installation/docker/ "link")  
# 安裝
全部功能套件皆已"一台機器docker方式安裝，須對外(也有非對外的方式)"  
### DCGM-export
安裝DCGM-export的套件，需要確定DCGM服務是開啟的  
```
sudo docker run -d --gpus all --rm -p 9400:9400 nvcr.io/nvidia/k8s/dcgm-exporter:2.0.13-2.1.2-ubuntu18.04
```
確認服務是否起來  
```
curl [DGX-Station IP]:9400/metrics
```
這邊額外建議除了監控GPU以外，另外安裝Prometheus的node監控  
可以用這個套件在Promethues看到CPU、Memory、I/O之類的資訊  
### 安裝prometheus  
由於我們塞入了兩個額外安裝的metrics，所以我們需要在安裝Promethues的Config  
額外添加路徑指向我們安裝的兩個metrics
```
sudo mkdir -p /etc/prometheus
cd prometheus/
```
Promethues的Config預設檔案可以從下方連結下載 
< https://github.com/prometheus/prometheus/blob/39d79c3cfb86c47d6bc06a9e9317af582f1833bb/documentation/examples/prometheus.yml >  
或是可以使用此資料夾內的檔案(需要修改成目前DGX的IP)
在`- targets :['localhost:9090']` 修改成`- targets :['localhost:9090' , 'DGXIP:9400' , 'DGXIP:9100']  `  
`9400`代表了DCGM-export  
`9100`代表了node-export  
如下圖所示  
![img](https://github.com/ReSin-Yan/DGX-Demo/blob/main/img/prometheusConfig%E7%AF%84%E4%BE%8B.png)  

修改好之後，在建置prometheus的時候將預設的config file複寫  
```
sudo docker run -p 9090:9090 -d -v /home/nvidia/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus 
```
#### 網頁檢查
< DGXIP:9090 >  

登入之後檢查是否有拿到metrics  
![img](https://github.com/ReSin-Yan/DGX-Demo/blob/main/img/Promethues%E6%AA%A2%E6%9F%A5.PNG)  

### 安裝 grafana  

```
sudo docker run -d -p 3000:3000 grafana/grafana
```
#### 網頁檢查
< DGXIP:3000 >  
![img](https://github.com/ReSin-Yan/DGX-Demo/blob/main/img/Grafana%E4%B8%BB%E9%A0%81.png)  
預設帳號密碼為
帳號:`admin` 密碼:`admin`


#### 設定資料來源
`左邊齒輪` > `Configuration` > `Data sources`  
![img](https://github.com/ReSin-Yan/DGX-Demo/blob/main/img/Grafana%20%E8%A8%AD%E5%AE%9A1.png)  

#### 選擇Promethues當作資料來源   
![img](https://github.com/ReSin-Yan/DGX-Demo/blob/main/img/Grafana%20%E8%A8%AD%E5%AE%9A2.png)  

#### 設定來源IP  
這個地方要注意的是，請勿輸入localhostIP  
原因是當前環境是Container建立出來的Grafana，所以在localhost會認成容器內部的路徑  
![img](https://github.com/ReSin-Yan/DGX-Demo/blob/main/img/Grafana%20%E8%A8%AD%E5%AE%9A2.png)  
點選Save & test 
看到綠色的Data source is working  
![img](https://github.com/ReSin-Yan/DGX-Demo/blob/main/img/Grafana%20%E8%A8%AD%E5%AE%9A4.png)  

#### Dashborad設定  
Dashboard可以自己建立或是匯入已經建立好的檔案  
Dashborad也類似於DockerHub，可以上傳或是下載套件  
<https://grafana.com/grafana/dashboards/12239>  
左邊 `+` > `Import`  > `import via grafana.com` > 輸入 `12339`  
![img](https://github.com/ReSin-Yan/DGX-Demo/blob/main/img/Grafana%20%E8%A8%AD%E5%AE%9A5.png)  

選擇來源的Promethues  
![img](https://github.com/ReSin-Yan/DGX-Demo/blob/main/img/Grafana%20%E8%A8%AD%E5%AE%9A6.png)  

確認服務
![img](https://github.com/ReSin-Yan/DGX-Demo/blob/main/img/Grafana%20%E8%A8%AD%E5%AE%9A7.png)  

#### 整合Node-export與DCGM-export  
在Dashboard匯入同資料夾下的檔案  
