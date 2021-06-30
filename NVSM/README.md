[參考文件](https://docs.nvidia.com/datacenter/nvsm/nvsm-user-guide/index.html "link")
# NVSM
這邊介紹幾個比較常用到的指令  

### show
顯示目前機器硬體的狀態
例如:GPU 風扇...等
```
sudo nvsm show health
```


最常使用的指令為
```
sudo nvsm show health
```
通過這行指令去檢查所有硬體的健康狀態  
可以使用 `>` 把檔案內容存下  
通常在開enterprise support case的時候都會需要此份文件  
也可以透過nvsm功能進行擷取  
```
sudo nvsm dump health
```
