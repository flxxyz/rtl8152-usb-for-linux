# rtl8152-usb-for-linux
螃蟹网卡 rtl8152/8156/8156(b)

仓库里有个 **r8152-2.16.3.tar.bz2** 可以用，你也可以去官网下载 [https://www.realtek.com/zh-tw/component/zoo/category/network-interface-controllers-10-100-1000m-gigabit-ethernet-usb-3-0-software](https://www.realtek.com/zh-tw/component/zoo/category/network-interface-controllers-10-100-1000m-gigabit-ethernet-usb-3-0-software)，自己视情况调整一些参数

## 安装编译依赖库

```sh
apt install linux-headers-$(uname -r) make build-essential
```

## 编译驱动

> 这里直接用仓库里的版本

解压驱动源码并进入文件夹

```sh
tar -jxvf r8152-2.16.3.tar.bz2 && cd r8152-2.16.3
```

开始编译安装并复制目录内的规则

```sh
make && make install
cp 50-usb-realtek-net.rules /usr/lib/udev/rules.d
```

## 开机加载驱动

```sh
depmod -a
update-initramfs -u

# 重启机器等待生效
reboot
```

## 检查驱动生效

```sh
lsmod | grep r8152
```

会有类似以下的输出

![screenshot-20230116-000200](https://user-images.githubusercontent.com/8678079/212552005-bb7fc0d2-0e10-4b76-b8ed-d0bb8d099df7.png)

### 检查驱动为r8152

> 网络接口名称可以通过 `ifconfig` 或 `ip addr` 查看前缀为 **enxc** 开头的

```sh
ethtool -i enxc84d44232258
```

![screenshot-20230116-000411](https://user-images.githubusercontent.com/8678079/212552137-a7e5024d-2781-41c3-9f12-3b0fbfb51e3c.png)

```sh
ethtool enxc84d44232258
```
可以看到支持的链接模式已经能够支持到 **2500M** 了哦~ 😄

现在手里没有支持2.5G速率的网线，问题不大，过年回家再试试。

![screenshot-20230116-002951](https://user-images.githubusercontent.com/8678079/212553410-30c39ef1-6adb-4f68-bddc-04d253626d6a.png)

