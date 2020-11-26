---
typora-root-url: ../images
---


# charles 抓包

## 下载

+ linux安装

> wget -q -O - https://www.charlesproxy.com/packages/apt/PublicKey | sudo apt-key add -1AD28806
>
> sh -c'echo deb https://www.charlesproxy.com/packages/apt/ charles-proxy main> /etc/apt/sources.list.d/charles.list'
>
> apt-get update
>
> apt-get install charles-proxy

+ Mac安装&Windows安装

1. 进入<https://www.charlesproxy.com/> 下载Windows安装包

2. 下载完成后安装




## IOS HTTP抓包

1. IOS与PC端处于同一网络

2. charles客户端点击Proxy - Proxy Settings  查看其中的HTTP Proxy值 Port的端口号

3. 查询本机地址

   > Alt+R  搜索CMD  输入ipconfig		#Windows查询IP地址
   >
   > 终端输入  ifconfig							#linux查询IP地址
   >
   > ifconfig | grep "inet"						#Mac查询本机地址

4. 在IOS端Wi-Fi - 进入与PC端同一wifi的设置中 - HTPP代理

5. 设置IOS HTTP代理为手动

   <img src="/IMG_1347.PNG" style="zoom: 40%;" />

6. 输入你的ip地址与端口号（默认8888）

7. 在app做任意操作，就可以在PC端查到它的网络请求了

   

## IOS HTTPS请求

1. 重复 IOS HTTP 步骤1-5

2. 在charles 中 HELP - SSL Proxying - Install Charles Root Certificate 安装根证书

3. 设置SSL代理 Proxy->SSL Proxying Settings - ADD  host中设置为"*"，Port设置为“443”

   ![](/Users/yunbosheng/Documents/mystudyblog/zj_blog/summarize/images/image-20201116142532559.png)

4. 准备给IOS安装证书 Help –> SSL Proxying–> Install Charles Root Certificate on a Mobile Device or Remote Browser

5. 在IOS端输入网址： http://charlesproxy.com/getssl

6. 安装描述文件

7. 查询你的IOS系统版本，如果为IOS10.3之后，需要设置->通用->关于本机->证书信任设置-> 找到charles proxy custom root certificate然后信任该证书

8. 在app做任意操作，就可以在PC端查到它的网络请求了

   

## Android HTTP请求

1. Android端与PC端处于同一网络

2. charles客户端点击Proxy - Proxy Settings  查看其中的HTTP Proxy值 Port的端口号

3. 查询PC本机地址

   > Alt+R  搜索CMD  在CMD中输入ipconfig	#Windows查询IP地址
   >
   > 终端输入  ifconfig				#linux查询IP地址

4. 在Android端WLAN进入与PC端同一wifi的设置中 - HTPP代理

5. 设置Android 代理为手动

6. 输入你的ip与端口号

7. 在app做任意操作，就可以在PC端弹出窗口，点击ALLOW

8. 现在开始抓包吧

   

## Android HTTPS 请求

1. 重复 'Android HTTP请求'的1-6步

2. 在charles 中 HELP - SSL Proxying - Install Charles Root Certificate 安装根证书设置SSL代理 Proxy->SSL Proxying Settings - ADD  host中设置为"*"，Port设置为“443”

   ![](/Users/yunbosheng/Documents/mystudyblog/zj_blog/summarize/images/image-20201116142532559.png)

3. 准备给Android安装证书 Help –> SSL Proxying–> Install Charles Root Certificate on a Mobile Device or Remote Browser

4. 在Android端输入网址： http://charlesproxy.com/getssl 或者上图的 chls.pro/ssl

5. 安装证书

   如果遇到无法安装的证书（如小米手机无法直接安装），建议不要用小米手机自带的浏览器下载crt文件，另下载下载一个第三方的浏览器，再登录第四步的网址下载证书

   更多设置 - 系统安全 - 从存储设备安装 安装下载的证书

6. 现在开始抓包吧

   

##  Android 6.0 以上抓包解决方案

0. 测试机型为小米9

1. 从github上下载需要的page

   > taichi ：https://github.com/taichi-framework/TaiChi/releases
   >
   > JustTrustMe .4 ：https://www.lanzous.com/i7skw8f

2. 使用``adb``命令或其他方式分别安装这两个pages

3. 安装完成后打开太极

4. 创建应用

   ![](/Users/yunbosheng/Documents/mystudyblog/zj_blog/summarize/images/1.gif)

   需要卸载原应用重新安装，不用担心，太极会帮你操作，只需要你按一按确定

   <img src="/Users/yunbosheng/Documents/mystudyblog/zj_blog/summarize/images/2.gif" style="zoom:50%;" />

5. 添加后，重复**Android HTTPS 请求**1～2步骤

6. 尝试进行抓包吧

   

## 修改请求和响应

1. 在app调用自己要操作的服务

2. 在charles中查看

   ![](/Users/yunbosheng/Documents/mystudyblog/zj_blog/summarize/images/81B825E5-2287-44F6-9F73-A06D16BDDF5F.png)

3. 右键单击要操作的服务，下拉菜单中找到"**breakpoins**"

5. 再次通过app调用该服务

6. charles弹出新页面

7. 修改请求

   ![](/Users/yunbosheng/Documents/mystudyblog/zj_blog/summarize/images/7BA0A65E-86BB-4432-AD83-AA2586118929.png)

8. 修改响应

   ![](/Users/yunbosheng/Documents/mystudyblog/zj_blog/summarize/images/36C5F588-AABB-40F8-A336-A52BDBC70617.png)