# android 系统

[android_C代码开发](https://github.com/Ewenwan/ShiYanLou/blob/master/MCU/arm/android_C代码开发.md)

[Android Skia2D引擎库 深度优化的算法、完善的渲染体系和精炼的代码框架](https://blog.csdn.net/jxt1234and2010/article/list/3?)

[Android图形显示系统](https://blog.csdn.net/jxt1234and2010/article/details/44503019)

[AndroidlibJpeg库解码OpenCL优化](https://blog.csdn.net/jxt1234and2010/article/details/45200441)

[adb 等工具](https://blog.csdn.net/wbdwsqwwn/article/details/25201779)


# Android ADB linux命令集合

A.用adb局域网功能连接设备 

     1，先用usb连接运行adb命令，将连接方式改为tcpip 
          adb tcpip {port}     port为端口号
          adb connect xxx.xxx.xxx.xxx(设备ip):port(刚才设置的端口号) 
          
     3，正常运行adb命令 
     
  adb的工作方式比较特殊采用监听Socket TCP 5554等端口的方式
  
     2，拔掉usb线，运行adb命令连接设备 
     
     让IDE和Qemu通讯，默认情况下adb会daemon相关的网络端口，所以当我们运行Eclipse时adb进程就会自动运行。 ADB是一个 客户端-服务器端 程序, 其中客户端是你用来操作的电脑, 服务器端是android设备. 

B.adb shell的一些常见命令 

　　1.adb shell 

　　      a.通过上面的命令，就可以进入设备或模拟器的shell环境中，在这个Linux Shell中，你可以执行各种Linux 的命令,如果只想执行一条命令，可以输入adb shell cmd 
　　         eg: adb shell dmesg会打印出内核的调试信息  
           
adb shell logcat v会打印出log信息 

　　   b.adb shell ls列出设备的目录列表 
    eg: adb连接设备操作 
    adb shell 
    adb -s xxxx shell 

　　2.上传文件: adb push 
  
　　  下载文件: adb pull 
    
　　   /tmp/...指的是在设备linux环境中要操作文件的路径 
     
　　     eg: adb push key data/app 就是将key文件上传到用户目录中 
       
  a.将文件放入设备 
  
         eg: adb push xxx.* /directory 
     adb -s xxxx(设备编号) xxx.* /directory 
     
     b.将文件拉出设备 
     
        eg: adb pull xxx.* /directory    
                adb -s xxxx(设备编号) xxx.* /directory 

　　3.安装程序: adb install <*.apk> 
  
　　  卸载软件: adb unistall apk(注意卸载的时候和安装的时候的文件名是不一样的，例如安装的时候adb shell GPSStatus2b2.apk,这个apk文件就被安装在data/app目录下，但是使用uninstall的时候，首先要到、data/app目录下查看安装的apk文件在linux目录下的文件名，发现是com.eclipsim.gpsstatus.apk，使用adb uninstall com.eclipsim.gpsstatus.注意不要加apk后缀。返回success结果证明文件卸载成功)。 
    
  a.用adb安装apk 
  
            eg: adb install xxx.apk 
                adb install -s xxxx(设备编号) xxx.apk     多个设备 
                
          b.用adb卸载apk 
          
         eg: adb uinstall xxx.apk(通常要写明详细的包名和activity名) 
         
     adb uinstall -s xxxx(设备编号) xxx.apk     多个设备 
     
　　补充一点，通过adb安装的软件(*.apk)都在"/data/app/"目录下，所以安装时不必制定路径。 
  
　　卸载的时候当然也可以直接到目录下使用rm命令也可。 

          如果有多个设备在运行的话，发送命令时必须用上-s,-e或-d这几个参数指定目标设备。 
                adb -e  发送命令到模拟器。 
                adb -d  发送命令到到USB设备，比如手机。 
                adb -s  指定一个目标。adb -s <serialNumber> <command>install <path-to-apk> 
          例如：adb -s emulator-5554 install helloWorld.apk 

　　4.显示android模拟器状态:


　　 adb devices 列出所有连接的设备 
   
          例如： 
                ~$ sudo /opt/android/android-sdk/tools/adb devices 
                List of devices       attached 
                emulator-5554      device 
                HT95LKF00945    device 
                这里就列出了两个设备，第一个是模拟器，第二个是手机。 
          注意：这里是用root用户来启动adb服务器和执行adb命令，不然就会 出现“no permissions” 
          
　　 adb get-serialno 打印设备序列号 
   
　 adb version 列出ADB的版本号 
  
　　 adb get-state 打印出的结果一般是offline | bootloader | device 
   
                adb help  查看adb所支持的所有命令 
                adb version    查看adb的版本序列号 
                adb logcat  打印日志到屏幕 
                adb bugreport 打印dumpsys,dumpstate和logcat数据到屏幕 
                adb jdwp       查看指定的设施的可用的JDWP信息. 
                adb forward    forward <local> <remote> 
                adb get-serialno 查看adb实例的序列号. 
                adb get-state 查看模拟器/设施的当前状态. 
                adb ppp 通过use设备运行PPP 
                adb wait-for-device  如果设备不联机就不让执行。 

　　5.等待正在运行的设备: adb wait-for-device 

　　6.adb start-server 

　　 adb kill-server 一般在键入adb shell命令后显示device offline或者是显示有多个设备的情况下使用 

　　7.adb remount 重新挂载系统分区，就是将系统分区重新挂载为可写。 

　　8. adb root使用管理员权限 

　　9. adb bugreport打印除所有的bug信息 

　　10.adb shell logcat -b radio 记录无线通讯日志：一般来说，无线通讯的日志非常多，在运行时没必要去记录，但我们还是可以通过命令，设置记录： 

　　11.adb emu 

　　12.端口转发: adb forward adb forward tcp:5555 tcp:1234 

　　(将默认端口TCP 5555转发到1234端口上) 
