# sonic-ios-bridge

本文为跨平台iOS调试工具sonic-ios-bridge（sib）的介绍与原理简述。 👉[Github地址](https://github.com/SonicCloudOrg/sonic-ios-bridge)

<a href="#">  
<img src="https://img.shields.io/github/stars/SonicCloudOrg/sonic-ios-bridge?style=social">
<img style="margin-left:10px" src="https://img.shields.io/github/forks/SonicCloudOrg/sonic-ios-bridge?style=social">
</a>

## 本仓库贡献者

<a href="https://github.com/SonicCloudOrg/sonic-ios-bridge/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=SonicCloudOrg/sonic-ios-bridge" />
</a>

## 介绍

> **sonic-ios-bridge** 是基于 [gidevice](https://github.com/electricbubble/gidevice) 作为底层iOS通信协议，在此基础上丰富了周边辅助功能，如自动挂载开发者镜像、wda安装检测、iOS型号映射、命令行直接使用等等。
> 以打造跨平台执行xctest、WebDriverAgentRunner、性能监听等等特色功能的命令行iOS调试工具。
>
> > Sonic组织也在持续将iOS通信的探索 **共建** 到gidevice上，以下是Sonic组织目前参与建设的提交：
> >1. 新增DiagnosticsRelay实现设备关机与重启。[feat: [DiagnosticsRelay] new functions Reboot Shutdown](https://github.com/electricbubble/gidevice/commit/ad436febc507a655ddd5de4720e6b0843bf45b16)
> >2. 新增SpringBoardServices与App图标获取。[feat: [SpringBoard] support get the app's icon](https://github.com/electricbubble/gidevice/commit/a31cdff57d0fc234acf4a57d6f707a7b67a23f8d)
> >3. 扩展SpringBoardServices获取屏幕旋转方向。[feat: [SpringBoard] support get the orientation of the interface](https://github.com/electricbubble/gidevice/commit/e787834515aabaacdf9208953625dd48af8d8514)
>
> 所以您可以使用sib脱离Mac进行 **跨端iOS自动化** 、**iOS设备通信**、**iOS设备测试** 。
>
> 无论是 gidevice 还是 tidevice ，主要原理是与usbmux通信。
> usbmux的作用是实现跨平台与iOS设备服务的通信。
> 在Mac上usbmuxd是苹果的一个服务，主要用于在USB协议上实现TCP连接。iTunes、Xcode都有用到了这个服务，所以Windows系统需要安装iTunes。

## 注意事项

> 已知设备部分功能需要 **挂载开发者镜像**，又因为执行xctest（包括wda）时检查挂载镜像会造成**阻塞**问题，所以sib目前在 **1.1.7版本开始** 只会在执行xctest（包括wda）前自动检查挂载状态与自动挂载，其余功能单独使用时，不再自动挂载开发者镜像，用户也可以使用 <a href="https://sonic-cloud.gitee.io/#/SIB?tag=sib-mount" target="_blank">这个指令</a> 自行挂载。
>
> 目前已知挂载状态变更如下：
> 1. 设备 **首次使用** 或 **重启** 后，挂载状态会被重置。
> 2. 挂载状态被重置前只要挂载**一次**，就不再需要挂载。
>
> ✨当然如果您有更好的方法解决这个问题，欢迎发起pr请求一同建设✨

## 快速使用

> 1. 选择下方 **PC对应的平台压缩包** 下载并解压到任意目录。如加速链接失效，请前往 <a href="https://github.com/SonicCloudOrg/sonic-ios-bridge/releases" target="_blank">这里</a> 下载
>
> > **Linux:**
> >
>  > 👉<a href="https://download.fastgit.org/SonicCloudOrg/sonic-ios-bridge/releases/download/v1.1.10/sonic-ios-bridge_1.1.10_linux_arm64.tar.gz" target="_blank">linux_arm64.tar.gz</a>
>  >
>  > 👉<a href="https://download.fastgit.org/SonicCloudOrg/sonic-ios-bridge/releases/download/v1.1.10/sonic-ios-bridge_1.1.10_linux_armv6.tar.gz" target="_blank">linux_armv6.tar.gz</a>
>  >
>  > 👉<a href="https://download.fastgit.org/SonicCloudOrg/sonic-ios-bridge/releases/download/v1.1.10/sonic-ios-bridge_1.1.10_linux_x86.tar.gz" target="_blank">linux_x86.tar.gz</a>
>  >
>  > 👉<a href="https://download.fastgit.org/SonicCloudOrg/sonic-ios-bridge/releases/download/v1.1.10/sonic-ios-bridge_1.1.10_linux_x86_64.tar.gz" target="_blank">linux_x86_64.tar.gz</a>
>
>  > **Macosx:**
>  >
>  > 👉<a href="https://download.fastgit.org/SonicCloudOrg/sonic-ios-bridge/releases/download/v1.1.10/sonic-ios-bridge_1.1.10_macosx_arm64.tar.gz" target="_blank">macosx_arm64.tar.gz</a>
>  >
>  > 👉<a href="https://download.fastgit.org/SonicCloudOrg/sonic-ios-bridge/releases/download/v1.1.10/sonic-ios-bridge_1.1.10_macosx_x86_64.tar.gz" target="_blank">macosx_x86_64.tar.gz</a>
>
>  > **Windows:**
>  >
>  > 👉<a href="https://download.fastgit.org/SonicCloudOrg/sonic-ios-bridge/releases/download/v1.1.10/sonic-ios-bridge_1.1.10_windows_arm64.tar.gz" target="_blank">windows_arm64.tar.gz</a>
>  >
>  > 👉<a href="https://download.fastgit.org/SonicCloudOrg/sonic-ios-bridge/releases/download/v1.1.10/sonic-ios-bridge_1.1.10_windows_armv6.tar.gz" target="_blank">windows_armv6.tar.gz</a>
> >
>  > 👉<a href="https://download.fastgit.org/SonicCloudOrg/sonic-ios-bridge/releases/download/v1.1.10/sonic-ios-bridge_1.1.10_windows_x86.tar.gz" target="_blank">windows_x86.tar.gz</a>
> >
>  > 👉<a href="https://download.fastgit.org/SonicCloudOrg/sonic-ios-bridge/releases/download/v1.1.10/sonic-ios-bridge_1.1.10_windows_x86_64.tar.gz" target="_blank">windows_x86_64.tar.gz</a>
>
> 2. 执行指令（Windows不需要，但是Windows需要安装iTunes）。
> ```
> sudo chmod 777 sib
> ```
> 3. 执行指令有输出版本号即可（Windows不需要./）。
> ```
> ./sib version
> ```
> 4. 🎉恭喜！您已经可以开始使用了！请移步到功能列表，如果Mac弹出安全弹窗，可以查看下方常见问题。
> 5. （附）如果想任意目录下都可以使用sib，需要将sib路径添加到系统环境变量PATH中。

## 名词解释

> 1. **flags** 意为当前指令可用的子命令，例：
> ```
> sib devices listen
> ```
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;那么 listen 就是 devices 命令的子命令。
>
> 2. **option** 意为当前指令可用选项，例如：
> ```
> sib devcies -d
> ```
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;那么 -d 就是 devices 命令的选项。

## 常见问题

> 1. 为什么有的版本的Agent的plugins目录下的是sonic-ios-bridge.exe而不是sib.exe？
>
> > 其实是同一个东西，只是Agent的我方便用户辨认，更改了文件名，实际上作用是一样的。
>
> 2. Windows上使用为什么报超时问题？
>
> > Windows没有自带usbmuxd，需要安装iTunes哦。
>
> 3. Mac上使用会有安全弹窗？
>
> > Mac：系统偏好设置 - 安全性与隐私 - 通用，点击信任或仍要打开。
>
> 4. 执行时报错 receive packet: InvalidService 之类的字样
>
> > 设备在首次使用或者重启后部分功能需要先挂载开发者镜像（除了运行xctest或wda会自动挂载外），可执行sib mount解决。
>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;更多疑问可前往👉[社区](https://sonic-cloud.wiki)👈交流

## 鸣谢

- [https://github.com/electricbubble/gidevice](https://github.com/electricbubble/gidevice)
- [https://github.com/libimobiledevice/libimobiledevice](https://github.com/libimobiledevice/libimobiledevice)

## 本文贡献者

<div class="cont">
<a href="https://gitee.com/ZhouYixun" target="_blank">
<img src="https://portrait.gitee.com/uploads/avatars/user/2698/8096045_ZhouYixun_1645499109.png!avatar100" width="50"/>
<span>ZhouYixun</span>
</a>
</div>


&nbsp; &nbsp;
***
不够详细？[点此](https://github.com/SonicCloudOrg/sonic-offical-website/edit/main/src/markdown/sib/re-sib.md) 发起贡献改善此页

