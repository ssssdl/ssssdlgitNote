# 关于最近使用ida的一些调试命令
```
#连接夜神模拟器
adb connect 127.0.0.1:62001 

#打开Android shell
adb shell

#向Android中传输文件，注意这里面Windows上的斜线和Android的斜线是相反的
adb push pc上的文件 Android上要存放的路径（一般使用/data/local/tmp）

#端口转发，将Android中的端口转发到Windows上，frida调试需要转发27042端口，ida转23946（好像试，具体可以看输出信息）
adb forward tcp:27042 tcp:27042

#debug启动app
adb shell am start -D -n  com.tencent.tmgp.sgame/.SGameActivity

#测试其他应用
adb shell am start -D -n com.cm.common/com.cm.healthmanage.activity.LoginActivity

#不知道这个应用mainactivity 
adb shell dumpsys  package com.cm.common

# 全局备份
adb backup -apk -shared -all -f E:\Desktop\backup.ap

# 从备份中恢复
adb restore E:\Desktop\backup.ap


# 将所有app开启debug
adb shell su
chmod 755 /data/local/tmp/mprop
data/local/tmp/mprop
setprop ro.debuggable 1
/data/local/tmp/mprop -r
```

# Android动态调试准备步骤
- 连接模拟器或者手机
- 将ida中的服务端程序传入客户机：`adb push D:\IDA_Pro_v7.0\dbgsrv\android_server /data/local/tmp`
- mprop可以将所有的程序的debug打开：`adb push E:\Desktop\mprop\libs\armeabi-v7a\mprop /data/local/tmp`
- 将这两个文件付755权限：`adb shell chmod 755 /data/local/tmp/android_server /data/local/tmp/mprop`
- 开启ida服务端的服务：`adb shell /data/local/tmp/android_server`
- 转发端口：`adb forward tcp:23946 tcp:23946`
- debug开启app：先查询主activity`adb shell dumpsys package com.cm.common`,然后debug模式启动`adb shell am start -D -n com.cm.common/com.cm.healthmanage.activity.LoginActivity`
- 查询这个app的PID:`adb shell ps|grep com.cm.common`
- 转发这个app进程：`adb forward tcp:8700 jdwp:PID`
- 打开ida：go进入主界面Debugger->Attach->Remote ARMLinux/Android debugger然后点击Debug options选上Suspend on process entry point、Suspend on thread start/exit、Suspend on library load/unload这三项，ok，然后ip填127.0.0.1因为是本地模拟器，数据线连接手机的话也一样，其他另说，ok，选中要调试的app的进程，ok
- 启动jdb：jdb -sourcepath .\src -connect com.sun.jdi.SocketAttach:hostname=localhost,port=8700

然后就可以动态调试了