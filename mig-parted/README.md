[參考文件](https://github.com/NVIDIA/mig-parted "link")    
# DGX設定
在轉換MIG Mode的時候，必須"確定"卡片上面沒有任何服務  
DGX Station預設會帶起相關的服務，在轉換前需要確保關閉  

透過以下指令去檢查對應卡片上面的服務(在DGX上，其中一張卡片為Display Card)
```
sudo lsof /dev/nvidia0
sudo lsof /dev/nvidia1
sudo lsof /dev/nvidia2
sudo lsof /dev/nvidia3
sudo lsof /dev/nvidia4
```
依序關閉以下服務  
關閉nvsm(可在mode切換完之後重新開啟服務)  
```
sudo systemctl stop nvsm
```
關閉dcgm(可在mode切換完之後重新開啟服務)  
```
sudo systemctl stop dcgm
```
關閉桌面服務(DGX Station預設會開啟桌面服務)  
```
sudo init 3
```

TIPs:nvidia-persistenced不需要關閉
# MIG 模式第一次開啟
注意這邊是以DGX為例子(跳過不具備MIG模式的Display Card)指定其中的第0,1,2,4張卡片轉換為MIG Mode
```
sudo nvidia-smi -i 0,1,2,4 -mig 1
```

# 安裝  
總共有三種安裝方式  
分別為  
1. Build form source  
2. Docker 
3. Go Get
這邊採用最為方便的安裝方式Docker(如果在其他平台需要額外安裝Docker)
網路需要對外  
透過docker執行
```
sudo docker run \
    -v $(pwd):/dest \
    golang:1.15 \
    sh -c "
    GO111MODULE=off go get -u github.com/NVIDIA/mig-parted/cmd/nvidia-mig-parted
    GOBIN=/dest     go install github.com/NVIDIA/mig-parted/cmd/nvidia-mig-parted
    "

```  
會在當前路徑產生一個nvidia-mig-parted執行檔
將此檔案複製進入環境變數內
```
sudo cp nvidia-mig-parted /usr/local/bin/
```



# MIG-Parted 使用
MIG-Parted總共有三種指令  
分別為 apply,export assert  
apply:指定yaml檔案將目前mig的分配方式按照指定檔按進行設定  
export:顯示幕前MIG分配詳情  
assert:指定yaml檔案跟目前mig的分配方式進行比較  

# MIG-Parted 範例
目前官方提供的exapme有包含DGX的範例，但是其為80GB的設定檔  
可以透過以下連結獲取40GB的設定檔，內部也是包含原有的80GB範例檔案  
```
git clone https://github.com/ReSin-Yan/mig-parted.git
```
