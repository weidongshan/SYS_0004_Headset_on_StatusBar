  
Add headset icon in statusbar from weidongshan's android video tutorial.  
  
用法:  
  
首先 cd android-5.0.2  
    
a. 替换、上传文件:
替换 frameworks/base/core/res/res/values/config.xml  
上传 stat_sys_headset_with_mic.png 和 stat_sys_headset_without_mic.png 到目录 frameworks/base/packages/SystemUI/res/drawable-hdpi  
替换 frameworks/base/packages/SystemUI/src/com/android/systemui/statusbar/phone/PhoneStatusBarPolicy.java  
  
b. 编译
b.1 
. setenv
lunch full_tiny4412-eng    

b.2 mmm frameworks/base/core/res    // 编译config.xml
    得到  out/target/product/tiny4412/system/framework/framework-res.apk
d.3 mmm frameworks/base/packages/SystemUI   // 编译图标文件, 编译源码
    得到 out/target/product/tiny4412/system/priv-app/SystemUI/SystemUI.apk

c. 替换单板上的文件

在ubuntu上:
   把上述2个apk文件放入/work/nfs_root目录

在开发板上执行:
   su
   mount -o remount /system 
   busybox mount -t nfs -o nolock,vers=2 192.168.1.123:/work/nfs_root  /data/nfs
   cp /data/nfs/framework-res.apk /system/framework/framework-res.apk
   cp /data/nfs/SystemUI.apk /system/priv-app/SystemUI/SystemUI.apk 

d. 测试
   使用含有驱动"DRV_0008_jack_for_sound_card"的zImage启动开发板,
   执行以下命令:
	 触发上报headset插入:   echo 4 1 > /sys/class/input/input0/test_input  
	 触发上报headset取下:   echo 4 0 > /sys/class/input/input0/test_input  
	 触发上报headphone插入: echo 2 1 > /sys/class/input/input0/test_input  
	 触发上报headphone取下: echo 2 0 > /sys/class/input/input0/test_input  
	 触发上报lineout插入:   echo 6 1 > /sys/class/input/input0/test_input  
	 触发上报lineout取下:   echo 6 0 > /sys/class/input/input0/test_input  
     