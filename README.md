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

安装完成后，可以看到是基于Qt5.12.4了（如果安装了两个版本的QtCreator，注意别打开错了）
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
将`libfcitxplatforminputcontextplugin.so`文件复制到`/home/HBA/qtcreator-4.9.2/lib/Qt/plugins/platforminputcontexts/`路径下  
重新启动该QtCreator就可以输入中文了（注意打开的是基于Qt5版本的QtCreator）  
可以使用麒麟操作系统自带的`搜狗输入法`输入中文    

### Qt应用程序输入中文
仅仅只是在QtCreator编译器里面还不行，需要在QtCreator编译好的Qt应用程序里面能输入中文才行    
将`libfcitxplatforminputcontextplugin.so`文件复制到`/home/HBA/Qt/5.15.2/gcc_64/plugins/platforminputcontexts`路径下  
再次编译应用程序，即可在Qt应用程序中输入中文

# Fcitx的编译
很多情况下，每个人的操作系统环境不同、Qt与QtCreator版本不同，都会导致编译出来的（.so）动态库不能通用    
此处介绍一下编译的方法

## 安装依赖环境
构建Fcitx动态库需要一些依赖环境，在构建之前需要安装好
```
sudo apt-get update
sudo apt install git
sudo apt install cmake
sudo apt install extra-cmake-modules
sudo apt install qtbase5-private-dev
sudo apt-get install libfcitx5utils-dev
sudo apt install fcitx-libs-dev
sudo apt-get install libxkbcommon-x11-dev
```

## 下载源代码
直接打开用户目录终端输入

    git clone https://github.com/fcitx/fcitx-qt5.git

之后可以在用户目录下找到下载的源代码（我的是在`/home/HBA/`下）    

## 进行配置
在`fcitx-qt5`目录下打开终端，输入设置Qt的环境变量（之后一直用该终端进行编译操作，不要关闭）：
```
export PATH=$PATH:/home/HBA/Qt/5.15.2/gcc_64/bin
```
（应该不需要修改，不过如果编译Qt6的话就需要）之后修改`fcitx-qt5`目录下的`CMakeLists.txt`文件
```
option(ENABLE_QT5 "Enable Qt5" On)
option(ENABLE_QT6 "Enable Qt6 im module" Off)
option(ENABLE_LIBRARY "Qt library" On)
```
## 编译
创建一个build目录，并且进入该目录

    mkdir build && cd build

执行cmake命令

    cmake ../

执行make

    make -j4

之后在`build`目录下的`/qt5/platforminputcontext`就有需要的`libfcitxplatforminputcontextplugin.so`插件动态库了。
    
之后的操作与`第三步`相同。
