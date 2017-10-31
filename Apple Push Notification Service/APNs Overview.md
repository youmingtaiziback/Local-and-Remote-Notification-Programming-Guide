# APNs Overview

app打开时，系统在app和APNs之间建立一个可信的、加密的、持久的IP连接

server和APNs之间建立持久的、安全的通道

如果发送远程通知时设备开机但没有运行，系统会展示通知。如果设备关机，APNs会稍后重试

## Provider Responsibilities

服务器的职能有：

* 接受token和其他app的数据
* 决定何时发送通知
* 向APNs发送通知请求

对于每一个发出去的通知请求：

* 创建一个包含通知内容的JSON字典
* 把通知内容、token和其他信息添加到HTTP/2请求里面
* 把HTTP/2请求发送给APNs，其中包括cryptographic credentials

## Using Multiple Providers

## Quality of Service, Store-and-Forward, and Coalesced Notifications

发送通知时，如果设备关机，则在APNs端新的通知会覆盖旧的。如果设备长时间不开机，存储在APNs端的所有通知都将被丢弃

通过在通知请求中添加_collapse identifier_可以实现通知的合并

## Security Architecture

APNs通过两个级别的信任机制实现端到端的加密验证和授权：_connection trust_和_device token trust_

_connection trust_服务器和APNs之间、APNs和app之前都工作

* 服务器到APNs：服务器端需要遵守苹果的推送通知协议



