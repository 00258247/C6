## 用紫光芯片漏洞，给讯飞C6刷类原生安卓

###### 本教程转载自 https://blog.moyuzj.cn/index.php/2025/08/20/%e8%bf%81%e7%a7%bb%e6%96%87%e7%ab%a0%e6%8a%98%e8%85%be%e7%ac%94%e8%ae%b0%ef%bc%9a%e7%94%a8%e7%b4%ab%e5%85%89%e8%8a%af%e7%89%87%e6%bc%8f%e6%b4%9e%e7%bb%99%e7%a7%91%e5%a4%a7%e8%ae%af%e9%a3%9ec6/

---

**刷机有风险，操作需谨慎！** 本教程只兼容 **安卓系统** 、紫光芯片的 C6 学习机！！！还有，解 BL 锁 **会清除平板所有数据！！！**

---

### 查看芯片类型

- 如果你是智慧课堂 4.0 用户，可以关闭屏幕，然后再打开，使其进入锁屏状态。在锁屏状态下按音量按键，在点击音量条下方的齿轮图标的同时从屏幕顶端快速往下滑（此方法概率失败，多试几次就好了）就可以打开通知栏。点击通知栏右上角进入设置，点击关于本机，如果下面的处理器包含 “Unisoc”，那么就是紫光芯片。

- 如果你是智慧课堂 5.0 用户，可以在主界面点击上方齿轮图标，再点击系统设置，按照上面进入设置后的步骤来就可以了。

---

### 解 **BL锁**

下载解压 `c6unlock.7z`文件找到`SPD_Driver_R4.20.4201.zip ` 解压后按照电脑版本安装对应的紫光的深刷驱动 （**Win11用户安装Win10文件也适用**）

然后运行 “DriverSetup.exe”，一路选是

![](https://blog.moyuzj.cn/wp-content/uploads/2025/01/1736993362-image.png)

如果看到这样的界面，那么就是安装成功了！

![](https://blog.moyuzj.cn/wp-content/uploads/2025/01/1736993395-image.png)

接下来，你需要双击打开 `config.reg`，导入注册表。

完成注册至此，驱动装好了。接下来，你需要下载这个 `kdxf-c6-unlock.zip`，这个压缩包就在开头的文件列表里。

把压缩包解压。先把你的学习机关机，然后长按音量下键（只按这个键！），等待 1 秒将平板插入电脑，然后再运行`unlock.bat`。

等待端口中显示`SPD U2S Diag `然后再按下回车键，不然就光跑代码，实际上不能解开BL锁。
接下来，如果遇到等待那就等它，不要按 Ctrl+C 结束进程，如果遇到 **按任意键继续**，那就敲下回车。

但，如果你的 cmd 窗口卡在这里：`Waiting for connection (300s)`，那么就要看看你的学习机是否成功进入深度刷机模式了！

#### BL解开

如果在执行完上面的 `unlock.bat` 后，cmd 消失，你的平板自己启动，在启动第一屏上显示 `“INFO: LOCK FLAG IS: UNLOCK!!! WARNNING: LOCK FLAG IS: UNLOCK, SKIP VERIFY!!!”` 并在稍后开始 “清除数据”，那么你的平板就成功解开BL锁了。

### 附：检查学习机是否连接电脑。

你可以右键桌面上的 “计算机” 或 “此电脑”，点击 “管理”（Win11找到显示更多选项，里面找到“管理”），左边一栏找到 “设备管理器”，在右边的分类里找到 **端口（COM 和 CPT）**。有SPD U2S Diag说明在深度刷机模式。

---

### 刷GSI

首先，下载 GSI！推荐使用 CrDroid，比较兼容 C6 学习机，只有一个自动亮度调节的问题（有解决方案），一个自动旋转屏幕问题（有替代方案）和一个网络连接受限问题（类原生国内通病，有完美解决方案）。

#### 注：

- 讯飞C6学习机的`system`分区只有3072mb（3gb）的大小。刷 gsi 镜像的时候要注意镜像大小必须小于 3072mb（3gb），否则大于 3072mb（3gb）的部分会无法刷入，然后因为系统文件缺失导致无限重启
- 讯飞C6学习机目前实验部分机子出现 crdroid 高版本刷入无法进入系统（卡第二屏）的情况。建议类Android 13系统 机器状况不尽相同，仅供参考

将`c6gsi.7z`下载解压后找到`spd_dump_stable_250131.zip `解压，找到`SPRD`文件。关机长按 音量 - 连电脑将平板进入深刷模式，待 **端口（COM 和 CPT）** 有`SPD U2S Diag`双击运行 `spd_dump.exe`

等待学习机连接成功后，在 `BROM >` 后依次输入一下命令：

```shell
fdl fdl1.bin 0x5500
fdl fdl2.bin 0x9efffe00
exec
w misc （此处拖入misc-wipe.bin文件）
w system （此处拖入你的GSI的IMG文件地址）
reset

```

`misc-wipe.bin`在`SPRD`上级文集目录里

依次执行完后，你的学习机会自动断开连接并重启清除 userdata。等到亮屏开机后，你的学习机就正式变为了类原生安卓 13！

接下来，你的屏幕可能会忽闪忽闪的，没关系，只需要赶紧过完 OOBE，在通知栏的控制中心里把自动亮度调节关掉就行。

---

### 修复问题：屏幕亮度

首先，进入 “设置”，下滑找到 “关于”，反复点击最下方的版本号 7 次打开开发者选项。然后返回设置主页，在最上方找到 “phh treble settings” 并点击进入。接着点击 “Misc Features”，下滑找到 BACKLIGHT 一栏，打开 “Force aternative backlight scae” 开关即可！

**注：关闭自动调光，不然你的平板还是会忽闪忽闪的。打开此开关后千万不要把亮度拉满，否则屏幕会比拉到最低还暗！拉到差不多靠近满亮度得了**

### 修复问题：屏幕旋转

这里可以使用一些依赖无障碍权限的屏幕方向管理器，如 Rotation。找到`com.nenya.rotation.apk`文件。可用adb调试写入平板中。

### 修复问题：解决 “网络连接受限” 的问题

从 Android 5.0（API 级别 21）开始，Android 设备就已能够检测 `Captive Portal` / 强制门户，并通知用户他们需要登录网络才能访问互联网。Android 原生系统用于检测的默认服务器是谷歌的，由于众所周知的原因无法访问。

虽然 WiFi 图标显示叹号和网络受限，但是访问网络问题不大，不过 Chrome 等应用应用会一直提示：无网络连接。

这会造成软件中需要网络连接的 `WorkManager` 一直不能执行

#### 解决方法：

删除变量＆关闭检测

```shell
adb shell settings delete global captive_portal_mode
adb shell settings put global captive_portal_mode 0 （注：Android 8 不需要执行这一条）

```

> 执行上述两条命令中，可能会出现  
> *daemon not running; starting now at tcp:5037  
> *daemon started successfully  
> 证明 adb 已经成功连接上（TCP 端口不一定相同），无影响。  
> 执行` adb shell settings get global captive_portal_mode`，返回结果应为`0`。  
> 认为设置 `captive_portal_mode` 为 `0` 是没有必要的，你都已经关闭检测了，为什么还需要设置 `URL` 呢？  
> 另外由于默认使用 `HTTPS`，所以 `HTTP URL` 也是不用配置的。  
> 所以只配置 `captive_portal_https_url` 就可以了。

此外，Android 开发者网站有内地站点：https://developer.android.google.cn/studio/releases/platform-tools/

**删除默认的强制门户设置：**

```shell
adb shell settings delete global captive_portal_https_url
adb shell settings delete global captive_portal_http_url
```

**修改新的设置:**

```shell
adb shell settings put global captive_portal_https_url https://connect.rom.miui.com/generate_204
adb shell settings put global captive_portal_http_url http://connect.rom.miui.com/generate_204
```

---

### 伪装学习机桌面 ###

找到`QZK5-STU等77个文件.rar`解压后，找到 `StuLauncher5_v5.6.5.2.xkt.apk`，`XunFeiServer_v1.0.0.26.apk`和`畅言学生登录_v5.0.10.0.xkt.apk`这三个个文件下载到平板上。

**注：`QZK5-STU等77个文件.rar` 中的大部分文件无法通过adb调试写入平板中，可以通过在平板中下载微信，QQ等软件将apk发到平板上安装。**

下载安装好，运行`畅言学生登录`这个应用。登录你的学生账号后，返回运行`StuLauncher5 `，就可以显示原来的学习机桌面了！

里面的大部分程序无法运行，用QZK5-STU.rar中的apk下载平板桌面后可以运行。

---

### 解决无法接受学校文件问题###

解压安装`StudyBridge(0)等3个文件.rar`

原程序视频 https://b23.tv/4mbGDJD

注册账号后，可绑定畅言学生账号

---

### 收尾

个人经历有限，错漏难免。
