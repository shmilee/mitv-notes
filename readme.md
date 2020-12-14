# adb 基础命令

```bash
$ adb version
Android Debug Bridge version 1.0.41
Version 30.0.5
Installed as /usr/bin/adb
$ adb devices [-l]
$ adb connect HOST[:PORT] # connect to a device via TCP/IP [default port=5555]
$ adb push [--sync] LOCAL... REMOTE
```

```bash
$ adb shell pm list packages -f # See their associated file.
$ adb shell pm list packages --user <USER_ID> # The user space to query.
$ adb install [-lrtsdg] [--instant] PACKAGE
$ adb uninstall  [-k] [--user <USER_ID>] PACKAGE
$ adb shell pm uninstall [-k] [--user <USER_ID>] PACKAGE
```

# MiTV info

* 开启【开发者模式】: 点击【设置】>【关于】>【产品型号】, 点击5次以上
* 开启【ADB调试】: 点击【设置】>【账号与安全】>【ADB调试】, 允许
* 电脑连接: `adb connect 192.168.x.x`, 电视**确认授权**

* CPU

```bash
$ adb shell cat /proc/cpuinfo
Processor	: AArch64 Processor rev 4 (aarch64)
processor	: 0
processor	: 1
processor	: 2
processor	: 3
Features	: fp asimd evtstrm aes pmull sha1 sha2 crc32 wp half thumb fastmult vfp edsp neon vfpv3 tlsi vfpv4 idiva idivt 
CPU implementer	: 0x41
CPU architecture: 8
CPU variant	: 0x0
CPU part	: 0xd03
CPU revision	: 4

Hardware	: Amlogic
Serial		: ********************
```

* GPU

```bash
$ adb shell dumpsys SurfaceFlinger |grep '^GLES'
GLES: ARM, Mali-450 MP, OpenGL ES 2.0
```

* ABI

```bash
$ adb shell getprop ro.product.cpu.abilist
armeabi-v7a,armeabi
```

* Android version

```bash
$ adb shell getprop ro.build.version.release 
6.0.1
```

# 应用管理

* packages

```bash
$ adb shell pm list packages > pkglist
```

## 删除系统应用

```bash
USER_ID=0
APPS=(
    com.miui.systemAdSolution #广告插件
    com.miui.tv.analytics #信息分析
    com.duokan.videodaily #视频头条
    #com.duokan.airkan.tvbox #米联投射服务
    com.xiaomi.mitv.shop #小米商城
    #com.xiaomi.tv.gallery #时尚画报
    com.xiaomi.mitv.payment #小米支付
    com.xiaomi.mitv.pay #电视支付
    com.xiaomi.mibox.gamecenter #游戏中心
    com.xiaomi.gamecenter.sdk.service.mibox #游戏中心服务安全
    com.xiaomi.mitv.tvpush.tvpushservice #电视推送
    com.mipay.wallet.tv #小米钱包
    com.xiaomi.mitv.upgrade #系统更新
    #com.xiaomi.mitv.appstore #应用商店
    com.xiaomi.smarthome.tv #米家
    #com.xiaomi.mitv.handbook #用户手册
    com.xiaomi.mitv.tvvideocall #视频通话
    com.mitv.alarmcenter #定时提醒
)
for app in ${APPS[@]}; do
    echo "Uninstall $app for user $USER_ID ..."
    adb shell pm uninstall --user $USER_ID $app
done
```

## 安装应用

* [Kodi](http://mirrors.kodi.tv/releases/android/arm/)

```bash
MYAPPDIR='myapks'
find $MYAPPDIR -type f -name '*.apk' -print -exec adb install {} \;
```

## Kodi IPTV

* 安装 `PVR IPTV Simple Client`
* 下载上传[IPTV直播源m3u8](http://www.kodiplayer.cn/movie/2898.html)
   ```bash
   M3U='20201214-江苏移动.m3u'
   $ adb shell mkdir -pv /sdcard/iptv
   $ adb push --sync ./$M3U /sdcard/iptv/
   $ adb shell ls /sdcard/iptv/
   ```
* 设置: 【插件】>【我的插件】>【PVR客户端】>【PVR IPTV Simple Client】>【设置】>【M3U播放列表路径】
