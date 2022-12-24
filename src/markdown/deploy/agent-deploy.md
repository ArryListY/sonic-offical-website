# Agent端部署
本文将介绍如何部署Agent端。

Agent有两种常见部署方式，分别为Docker部署和jar部署。
<el-alert style="margin-top: 20px" title="注意" show-icon type="info" description="谨记一个主机只能部署一个Agent端，每个Agent的Key不能重复使用，多台设备可接入同一Agent" :closable="false"/>

## 一、Docker部署 **（注：仅Ubuntu可用！可以点击 [这里](https://sonic-cloud.wiki/d/1255-agentsoniclinux) 查看官方推荐主机）** 
该方式将一次性部署Agent端以及所需环境。

准备工作：Docker，Sonic前后端部署完毕
> 1. 从部署好的前端界面【设备中心】的【Agent中心】新增Agent，记录Agent的Key。
>
> 2. <a href="https://gh.flyinbug.top/gh/https://github.com/SonicCloudOrg/sonic-agent/releases/download/v2.1.2/docker-compose.yml" target="_blank">点击这里</a> 下载最新的docker-compose.yml，参考注释修改里面的内容。（如加速链接失效，请自行前往 <a href="https://github.com/SonicCloudOrg/sonic-agent/releases" target="_black">这里</a> 下载）
>
> 3. 执行以下指令（自行根据提示更改参数）。
>
> ```
> docker-compose up -d
> ```
> 如果您为中国大陆用户，我们 **建议配置加速镜像源** 或 <a href="https://gh.flyinbug.top/gh/https://github.com/SonicCloudOrg/sonic-agent/releases/download/v2.1.2/docker-compose-zh.yml" target="_blank">点击这里</a> 下载docker-compose-zh.yml后执行以下指令直接使用加速镜像（后续down的时候需要docker-compose -f docker-compose-zh.yml down）
> ```
> docker-compose -f docker-compose-zh.yml up -d
> ```
> 
> 4. 部署完毕！自行插入设备即可。
> 
> 5. （附）如果您对Docker不熟悉，更推荐使用jar方式部署。


## 二、jar方式部署
该方式将以本地jar包部署Agent端、Appium等等环境。

> 0. **已知部分JDK出现不兼容的问题，Sonic官方推荐使用JDK15**，可以前往 [这里](https://docs.aws.amazon.com/corretto/latest/corretto-15-ug/downloads-list.html) 安装下载。
> 1. 从部署好的前端界面【设备中心】的【Agent中心】新增Agent，记录Agent的Key。
> 2. （如不需要接入安卓设备可跳过）将安卓SDK设置到系统环境变量，命名为ANDROID_HOME。打开SDKManager，下载 **platform-tools**。确保platform-tools目录存在，adb指令可用。
> 3. 将ANDROID_HOME、ANDROID_HOME/platform-tools添加到系统PATH中。
> 4. 选择 **PC对应的平台zip** 下载并解压到任意目录（标记为 **工作目录** ，**如以下加速链接失效**，请自行前往 <a href="https://github.com/SonicCloudOrg/sonic-agent/releases" target="_blank">这里</a> 下载）
> 
> > **Linux:**
> > 
>  > 👉<a href="https://gh.flyinbug.top/gh/https://github.com/SonicCloudOrg/sonic-agent/releases/download/v2.1.2/sonic-agent-v2.1.2-linux_x86.zip" target="_blank">sonic-agent-v2.1.2-linux_x86.zip</a>
>  >
>  > 👉<a href="https://gh.flyinbug.top/gh/https://github.com/SonicCloudOrg/sonic-agent/releases/download/v2.1.2/sonic-agent-v2.1.2-linux_x86_64.zip" target="_blank">sonic-agent-v2.1.2-linux_x86_64.zip</a>
>  > 
>  > 👉<a href="https://gh.flyinbug.top/gh/https://github.com/SonicCloudOrg/sonic-agent/releases/download/v2.1.2/sonic-agent-v2.1.2-linux_arm64.zip" target="_blank">sonic-agent-v2.1.2-linux_arm64.zip</a>
> >
> > **Macosx:**
> >
>  > 👉<a href="https://gh.flyinbug.top/gh/https://github.com/SonicCloudOrg/sonic-agent/releases/download/v2.1.2/sonic-agent-v2.1.2-macosx_x86_64.zip" target="_blank">sonic-agent-v2.1.2-macosx_x86_64.zip</a>
>  > 
>  > 👉<a href="https://gh.flyinbug.top/gh/https://github.com/SonicCloudOrg/sonic-agent/releases/download/v2.1.2/sonic-agent-v2.1.2-macosx_arm64.zip" target="_blank">sonic-agent-v2.1.2-macosx_arm64.zip</a>
> >
> > **Windows:**
> >
>  > 👉<a href="https://gh.flyinbug.top/gh/https://github.com/SonicCloudOrg/sonic-agent/releases/download/v2.1.2/sonic-agent-v2.1.2-windows_x86.zip" target="_blank">sonic-agent-v2.1.2-windows_x86.zip</a>
>  > 
>  > 👉<a href="https://gh.flyinbug.top/gh/https://github.com/SonicCloudOrg/sonic-agent/releases/download/v2.1.2/sonic-agent-v2.1.2-windows_x86_64.zip" target="_blank">sonic-agent-v2.1.2-windows_x86_64.zip</a>
> 
> 5. 赋予 **工作目录** 所有权限，然后确保解压后的mini、config、plugins文件夹与jar同级
> 
> ```
> $ sudo chmod -R 777 xxxxx
> ```
> 
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;然后可以列出文件夹检查一下。
> ```
> $ cd xxxxx    
> $ tree 
> 
> │  sonic-agent-xxxx.jar
> │
> ├─config
> │      application-sonic-agent.yml
> ├─plugins
> ├─mini
> ```
> 6. 修改config文件夹中 **application-sonic-agent.yml** 的配置信息，保存。
> 7. 在 **工作目录** 路径下执行以下指令。
>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**快速启动** （*注意！如果你是windows用户，请先在控制台输入 **chcp 65001** 并回车，再输入以下指令*）
> ```
> java -Dfile.encoding=utf-8 -jar sonic-agent-xxxx.jar
> ```
> 8. 部署完毕！自行插入设备即可（设备请竖直放置或平摊放置，左右旋转放置有可能影响坐标定位）。

## 常见问题

> 1. 明明配置好了ANDROID_HOME，并且adb可用，为什么还是检测不到ANDROID_HOME？
>
> >需要配置好ANDROID_HOME之后，PATH里面也需要配置好。确认echo %ANDROID_HOME% (win) 或 echo $ANDROID_HOME (mac 或 linux) 输出正确。
> 2. 查看日志发现与Server没有连上，该怎么解决？
> >主要分为多种情况:
> >1. Key配置不正确，一个Key只能一个Agent使用。
> >2. 所有ip不能使用localhost、127.0.0.1之类的配置。
>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;更多疑问可前往👉[社区](https://sonic-cloud.wiki)👈交流

## 本文贡献者
<div class="cont">
<a href="https://github.com/ZhouYixun" target="_blank">
<img src="https://avatars.githubusercontent.com/u/56339314?v=4" width="50"/>
<span>ZhouYixun</span>
</a>
<a href="https://gitee.com/soniclei" target="_blank">
<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEsAAABLCAYAAAA4TnrqAAAAAXNSR0IArs4c6QAABHZJREFUeF7tm9tXIkcQxj8EATcqCnIRRVQWdc0m+f8f8pSHvO3DZt1VXJSr3FXuwnLJqVmHiMcNXcrpMTnVLz7w9UzXb77uqao52n7/488xZCgRsAksJU6GSGCpsxJYDFYCS2BxCDC0cmYJLAYBhlScJbAYBBhScZbAYhBgSMVZAotBgCEVZwksBgGGVJwlsBgEGFJxlsBiEGBIxVkCi0GAIRVnCSwGAYbUUmfZ7XZshQLw+7x4s7QEh8MOm802Wf5wOEL/Wx/1RguFUhk39QYjtPlLLYMV349iM+CHw+FQimo8HqPV7iCZzuL65lZpzrxF2mGRm94fxuFd90y5SDWwb4MBLjM55K6KqlPmptMO6yC2i61QcArUYDBEs91GvdFEp9s1grMv2OFZXcHqyjKW3K4pPQFLJC9RqtTmBkLlQlphra+t4vjgLVxOp7E22lrV2g0+nycxHA5/uN5wMIC9nW24XN/n0bhtNPHh44lKjHPTaIW1H41gZzuMhftD/Oa2jo9fEv8Kyow06PfhILaHxfszbjAY4CyZQqlSnRuMWRfSCusovg9yCY3RaGScPenc1aw1Tn7/7fgIPu/axJXZqyK+XqaV579UaBksSguSqQxyBfWD2nQmxmNjCxfKFSSSqZcyUJ6vFZZxuG+GYGZS5WoNn07PlRdrtVArrKB/A4ex3UluNRqPUSpXcZHJotfrW81i5v21wqLVvD+KI7Dhm1rYcDQyEs5q7RrFSvXVgtMOi17/7+IxeNc8Tz5JOosGwyE6na5R3lBq0Wi1Zj51HQLtsCgoyuJj0QhCgQ2lcofShEarjXyhhErtWgeXJ+9hCSxzJeQyqg9pW75ZcmNhYWEmiGarbVl9aCmsx2T8G1741tfgWVmB2+2C/QfwKNvP5AtGnqZzvCpYjwMncASQzje3yzX1M23NZCqLfLGkjderhvWQAmX+0ciWUVSbg7bkh78+K5VL8yD6n4FFwZLTqGQyC3HqVpwZ3Qc99aE2WL8eH2Lds2q0Wig9SFykje4nd/x8FEfwPk+jpDaTu8JFOsu9zLP02mA9LKLp/1/yheKz6rqpYvz/Ciu6HTZ6UmZ60O508ek0AfrLGQ87D88pxjn3eqzV5izPyjKOD+OTA1q18fdwwdRhje1GJonsXa+Hk9Nz1Jt6MnxtsCjot3tRRMKhqRYxvdFS2fzMzJycubMVxuLi9w8ctJWL5Qq+JJIvMQtrrlZYVOb88u7gybrwrtdHs9VCo9lGr98zgnA5XfCsLhtJqgnJjI4K75Ozc/Y2ZtF5JNYKywAwo5BWCabTvcP5RQo1zZ/EtMMyYdC2ioQ34XQuqvAxNNTKoS7E11TakjaOZbBMQuFQwCikfzK+SDtgt/9TTNNLgABRY5BcRHkZ9+2p/CQUhJbDUljjq5EILMajEFgCi0GAIRVnCSwGAYZUnCWwGAQYUnGWwGIQYEjFWQKLQYAhFWcJLAYBhlScJbAYBBhScZbAYhBgSMVZAotBgCEVZwksBgGGVJwlsBgEGNK/AbDkeexTOAtZAAAAAElFTkSuQmCC" width="50"/>
<span>soniclei</span>
</a>
</div>


&nbsp;
&nbsp;
***
不够详细？[点此](https://github.com/SonicCloudOrg/sonic-offical-website/edit/main/src/markdown/deploy/agent-deploy.md) 发起贡献改善此页

