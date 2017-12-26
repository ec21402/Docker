
主要是希望能夠以docker為基底製作出一個能使用laravel的環境，並記錄一些重點讓自己在忘記時可以想起來。以下內容皆為使用windows 10 home。  

# 安裝

Docker提供了 [docker for windows](https://docs.docker.com/docker-for-windows/install/#download-docker-for-windows/) 專門給Windows Pro 10以上的主要的差別在於OS中有無 Hyper-V 。
[docker toolbox](https://docs.docker.com/toolbox/overview/) 給其他版本使用。
由於筆者目前是使用windows 10 home 所以並不做多餘的介紹其餘安裝方法可至docker 官網查詢。

## 開啟
docker for window 是有GUI介面的可直接使用，這邊筆者不多做介紹。  
docker toolbox 安裝完會有一個 Docker Quickstart Terminal 在桌面上開啟即可使用，開啟後會得到一個IP，此IP用途稍後介紹。
```
//一開始的IP如果忘了可下指令取得
docker-machine ip
```

# Image

在Docker Hub上有提供各種的image可供使用者下載，也可以自己製作。
```
//下載image
docker pull imagename
```
```
//查詢docker中的image
docker images
```
![](https://i.imgur.com/h47Mku4.jpg)  

# container 
將image創建成容器之後才能真正的使用。每重新執行就會重新建立都是全新的。
```
//查詢docker執行中的container
docker ps
```
![](https://i.imgur.com/bvqlKWQ.jpg)  
加上-a 列出所有的container
```
//創建新的容器
docker run --name forlaravel -d -p 8080:80 -v $PWD:/var/www/html php:7.0-apache
```
--name:給容器一個名子。
-d : 在後台執行。   
-p : 使容器的80 port 映射到本機端的8080port。
-v : $PWD指定的資料夾將會與Termimal目前的位置同步。

完成後使用exec指令進入容器。  
```
docker exec forlaravel/id bash
```
# laravel 
安裝laravel可以直接從github上clone下來放進同步的資料夾中，或是在進入容器中直接使用composer下載，在這途中會遇到許多liunx使用者權限的問題。  
不管直接clone或是使用composer安裝laravel都是需要安裝composer和git的。

跟著以下步驟即可 :
* install composer(linux載點)  
curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
* apt-get install git   
* apt-get update     
* php composer update  
  
沒意外的話完成後就可以在瀏覽器上打上一開始得到的ip加上:8080/public就會連到laravel預設的首頁，上面應該是會有錯誤的首先解決的第一個錯誤是evn.example  將他改為.evn之後再下指令更改APP_KEY，完成就可以看到laravel的首頁了。   
附上 點燈坊 詳細的設定步驟。
  
![點燈坊:如何重建由gitHub下載的Laravel專案](http://oomusou.io/laravel/laravel-clone-from-github/)

以上是最基礎的用法，實際上應該要分開建置。

