---
title: "Alpine"
description: "安装内核和 v2rayA"
lead: "v2rayA 的功能依赖于 V2Ray 内核，因此需要安装内核。"
date: 2020-11-16T13:59:39+01:00
lastmod: 2020-11-16T13:59:39+01:00
draft: false
images: []
menu:
  docs:
    parent: "installation"
weight: 15
toc: true
---

## 安装 V2Ray 内核 / Xray 内核

{{< alert icon="👉" text="如果你已经安装了内核，可以跳过此节。" />}}

V2Ray 安装参考：<https://github.com/v2fly/alpinelinux-install-v2ray>

Xray 安装参考：<https://github.com/XTLS/alpinelinux-install-xray>

## 安装 v2rayA

### 1. 下载二进制可执行文件

根据你的平台，从 [Release](https://github.com/v2rayA/v2rayA/releases) 获取具有 `v2raya_linux_xxx` 字样的无后缀名文件，并将其重命名为 `v2raya`，再把 `v2raya` 移动到 `/usr/local/bin` 并给予可执行权限。

  示例：
   ```
   wget https://github.com/v2rayA/v2rayA/releases/download/v1.4.4/v2raya_linux_x86_v1.4.4 -O v2raya
   mv ./v2raya /usr/local/bin/ && chmod +x /usr/local/bin/v2raya
   ```

### 2. 创建服务文件

在 `/etc/init.d/` 目录下面新建一个名为 `v2raya` 的文本文件，然后编辑，添加内容如下：

   ```ini
   #!/sbin/openrc-run
   
   name="v2rayA"
   description="A Linux web GUI client of Project V which supports V2Ray, Xray, SS, SSR, Trojan and Pingtunnel"
   command="/usr/local/bin/v2raya"
   command_args="--webdir=/usr/local/etc/v2raya/web --config=/usr/local/etc/v2raya"
   pidfile="/run/${RC_SVCNAME}.pid"
   command_background="yes"
   
   depend() {
   	need net
   }
   ```

保存文件，然后给予此文件可执行权限。

### 3. 安装 iptables 模块 并放行2017端口

  ```bash
  apk add iptables ip6tables
  ```
  ```bash
  /sbin/iptables -I INPUT -p tcp --dport 2017 -j ACCEPT
  ```

### 4. 运行 v2rayA 并开机启动（可选）

  ```bash
  rc-service v2raya start
  rc-update add v2raya
  ```
