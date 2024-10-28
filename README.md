# 基于麒麟操作系统的Qt5中文输入
本人的环境:  
* 操作系统：Kylin-Desktop-V10-SP1-2403-HWE-Release-20240430-x86_64
* Qt版本：6.8.0与5.15.2
* QtCreator版本：14.0.2与4.9.2

## 第一步：下载Qt
麒麟操作系统相当于Linux环境，所以需要下载Linux系统下的Qt-Online-installer：  [下载链接](https://d13lb3tujbc8s0.cloudfront.net/onlineinstallers/qt-online-installer-linux-x64-4.8.1.run)  

下载完成后，在该.run文件所在目录中打开终端，输入
```
chmod +x qt-online-installer-linux-x64-4.8.1.run
```
设置可执行权限，之后输入
```
./qt-online-installer-linux-x64-4.8.1.run
```
运行Qt安装程序
    
在选择Qt需要安装的组件时，需要选择QT5.15.2，其余可以根据具体情况来。可以同时安装多个版本的Qt与QtCreator。
    
## 第二步：下载旧版本的QtCreator
麒麟操作系统自带的gcc相关依赖库版本比较低，无法支持基于Qt6的QtCreator的Fcitx插件的编译  
因此需要自行下载低版本的QtCreator
    
### QtCreator版本查看
![image](https://github.com/user-attachments/assets/80a1587a-a691-4e9b-9096-e143b2bbfa5b)
    
主要看`Based On QtX.X.X`
