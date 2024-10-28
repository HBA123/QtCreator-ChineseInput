# 基于麒麟操作系统的Qt5中文输入插件
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
因此需要自行下载低版本（基于Qt5）的QtCreator
    
### QtCreator版本查看
![image](https://github.com/user-attachments/assets/80a1587a-a691-4e9b-9096-e143b2bbfa5b)
    
主要看`Based On QtX.X.X`，这个的版本号就代表QtCreator基于的Qt版本，上图所示就是基于Qt6版本的  
而我们需要的是基于Qt5版本的，所以需要自行去下载旧版本的QtCreator

### 基于Qt5的QtCreator下载
[下载链接](https://mirrors.ustc.edu.cn/qtproject/archive/qtcreator/4.9/4.9.2/qt-creator-opensource-linux-x86_64-4.9.2.run)  
下载的.run文件还是与之前操作类似，按步骤安装完成即可  

下载完成后，可以看到是基于Qt5.12.4了
![image](https://github.com/user-attachments/assets/cc986be7-0170-4920-9090-8e21da67acd4)


### 配置QtCreator
上述方法下载的QtCreator可能没有自动配置好，也就是没有与Qt链接，无法对代码进行构建与运行  
需要在 `工具` -> `选项` -> `Kits`中进行手动配置  
点击 `添加` ，在`名称`一栏输入
```
Desktop Qt %{Qt:Version} GCC 64bit
```
其余的应该就自动识别了，具体如下图所示：
![image](https://github.com/user-attachments/assets/632dda45-9bb6-4a6b-b578-b1f18261a291)

设置完成后记得点击`Apply`和`OK`    
    
## 第三步：复制粘贴
### QtCreator输入中文
如果是按照上述操作来进行，并且操作系统环境与我一致，应该就可以直接复制我编译好的Fcitx动态库（.so）文件进行使用    
将libfcitxplatforminputcontextplugin.so文件复制到/home/HBA/qtcreator-4.9.2/lib/Qt/plugins/platforminputcontexts/路径下  
重新启动该QtCreator就可以输入中文了
