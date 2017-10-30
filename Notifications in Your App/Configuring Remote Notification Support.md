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



