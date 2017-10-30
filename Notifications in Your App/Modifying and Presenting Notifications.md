# Modifying and Presenting Notifications

notification service app extension：改变内容

notification content app extension：改变样式

## Modifying the Payload of a Remote Notification

service extensions可以实现：

* 数据解密
* 下载媒体资源并作为附件加到通知里 
* 改变通知的title或者body
* 添加进程标识符或者修改通知的[`userInfo`](https://developer.apple.com/documentation/usernotifications/unnotificationcontent/1649869-userinfo)字典

在\[UNNotificationServiceExtension didReceiveNotificationRequest:withContentHandler:\]中如果不及时调用handler，系统将展示原始的通知

为了支持修改通知内容，服务器端创建远程通知时需要：

* mutable-content设为1

* 包含一个alert字典，字典中需要制定alert的title和body

## Presenting Notifications Using a Custom Interface on iOS

notification content app extension支持特定分类的本地和远程通知。本地通知通过`categoryIdentifier`指定分类；远程通知中，服务器会向通知中加一个aps字典，aps字典中包换`category`key

一个app中可以包含多个notification content app extensions，系统只允许一个category包含在一个extension中。

