# APNs Overview

app打开时，系统在app和APNs之间建立一个可信的、加密的、持久的IP连接

server和APNs之间建立持久的、安全的通道

如果发送远程通知时设备开机但没有运行，系统会展示通知。如果设备关机，APNs会稍后重试

## Provider Responsibilities

服务器的职能有：

* 接受token和其他app的数据
* 决定何时发送通知
* 想APNs发送通知请求

## 



