[NVIDIA GPU Cloud(NGC)](https://ngc.nvidia.com/ "link")  
[NVIDIA GPU Cloud(NGC) private registry user guide](https://docs.nvidia.com/ngc/ngc-private-registry-user-guide/ "link")  
 
# NVIDIA GPU Cloud 上傳使用範例 
在購買DGX系列或是NGC support service之後，會經由mail收到NVIDIA的權利證書(PDF)  
透過此權利證書可以進行NGC帳號申請  
可以分為  
選擇現有帳號以及新建新的帳號  
![img](https://github.com/ReSin-Yan/DGX-Demo/blob/main/img/NGC%E8%A8%BB%E5%86%8A.png)  
### 登入NGC
透過以下網址[NVIDIA GPU Cloud(NGC)](https://ngc.nvidia.com/ "link")進行登入  
輸入帳號密碼之後可以看到  
![img](https://github.com/ReSin-Yan/DGX-Demo/blob/main/img/NGC1.PNG)  
左方工作列表對比於一般NGC user來說多了Private Registry功能  

### 註冊Team  
NGC帳號分為 
`組織`  以及  `團隊 ` 
其中`組織`通常為domain，代表一整間公司，例如TSMC or Foxconn
組織之中可以透過 `團隊 ` 來進行細分  
同一個團隊可以彼此分享各自的內容  

左方工作列表 `ORGANIZATION` > `Teams` > 右下角 `+` > `Create Teams`  
![img](https://github.com/ReSin-Yan/DGX-Demo/blob/main/img/NGC2.PNG)  

建立完成之後可以在右上角進行Teams切換  
![img](https://github.com/ReSin-Yan/DGX-Demo/blob/main/img/NGC3.PNG)  

###  環境登入NGC  
NGC是一個位在雲端的私有倉庫雲  
使用上噓要進行登入的動作  
右上角使用者 `setup`  
![img](https://github.com/ReSin-Yan/DGX-Demo/blob/main/img/NGC4.PNG)  
選擇`Generate API Key` > `Get API Key`  
進入之後點選右上角 `Generate API Key` > 選擇 confirm  
![img](https://github.com/ReSin-Yan/DGX-Demo/blob/main/img/NGC5.PNG)  
之後在server client輸入以下訊息

```
sudo docker login nvcr.io

Username: $oauthtoken
Password: [your API Key]
```
輸入完成之後可以看到Login succesed的訊息

TIPs:每一組不同的Teams都會有不同的key

###  下載NGC的images  
NVIDIA的images接可以透過此種方式進行下載  
可以從左方工具列`CATALOG` > `Container` 找到符合自己需求的容器映像檔  
這邊以CUDA為例  
```
sudo docker pull nvcr.io/nvidia/cuda:11.3.1-base-ubuntu20.04
```

###  上傳NGC的images  
可以將自己建立的images上傳至NGC保存  
```
sudo docker tag nvcr.io/nvidia/cuda:11.3.1-base-ubuntu20.04 nvcr.io/[組織名稱]/[團隊名稱]/[映像檔名稱]:[容器Tag]
sudo docker push nvcr.io/[組織名稱]/[團隊名稱]/[映像檔名稱]:[容器Tag]
```

###  查找上傳NGC的images  
可以從左方工具列`PRIVATE REGISTRY` > `Container`  進入
根據使用者權限可以看到不同的映像檔  
![img](https://github.com/ReSin-Yan/DGX-Demo/blob/main/img/NGC6.PNG)  
