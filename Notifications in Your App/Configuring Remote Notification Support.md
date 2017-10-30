# Configuring Remote Notification Support

支持远程通知需要的步骤：

1. 开启远程通知
2. 注册到APNs并获取app-specific device token
3. 把device token发送给服务器
4. 实现处理远程通知的代码

> 注意
>
> APNs向非运行的app发送远程通知要求app必须启动过
>
> iOS设备上，如果用户通过多任务界面强制杀死app，app不会接收远程通知，知道再次打开

## Enabling the Push Notifications Capability

没有entitlements的app审核时会被拒接，测试时向APNs注册时返回错误

## Registering to Receive Remote Notifications

每次启动app时，必须和APNs注册，步骤：

* app向APNs注册
* 成功时APNs向设备放回token
* 系统通过代理方法向app放回token
* app把token发送到服务器 



