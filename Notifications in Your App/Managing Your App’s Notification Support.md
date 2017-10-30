# Managing Your App’s Notification Support

支持本地和远程通知必须在app启动时配置

## Requesting Authorization to Interact with the User

```
UNUserNotificationCenter* center = [UNUserNotificationCenter currentNotificationCenter];
[center requestAuthorizationWithOptions:(UNAuthorizationOptionAlert + UNAuthorizationOptionSound)
   completionHandler:^(BOOL granted, NSError * _Nullable error) {
      // Enable or disable features based on authorization.
}];
```

此方法仅在第一次被调用时弹出提示

## Configuring Categories and Actionable Notifications

用户通过点击Actionable notifications的按钮直接打开app

每个通知支持最多四个动作

#### Registering the Notification Categories for Your App

```
UNNotificationCategory* generalCategory = [UNNotificationCategory
     categoryWithIdentifier:@"GENERAL"
     actions:@[]
     intentIdentifiers:@[]
     options:UNNotificationCategoryOptionCustomDismissAction];

// Register the notification categories.
UNUserNotificationCenter* center = [UNUserNotificationCenter currentNotificationCenter];
[center setNotificationCategories:[NSSet setWithObjects:generalCategory, nil]];
```

不包含category的通知，显示时没有自定义按钮

#### Adding Custom Actions to Your Categories

```
UNNotificationCategory* generalCategory = [UNNotificationCategory
      categoryWithIdentifier:@"GENERAL"
      actions:@[]
      intentIdentifiers:@[]
      options:UNNotificationCategoryOptionCustomDismissAction];

// Create the custom actions for expired timer notifications.
UNNotificationAction* snoozeAction = [UNNotificationAction
      actionWithIdentifier:@"SNOOZE_ACTION"
      title:@"Snooze"
      options:UNNotificationActionOptionNone];

UNNotificationAction* stopAction = [UNNotificationAction
      actionWithIdentifier:@"STOP_ACTION"
      title:@"Stop"
      options:UNNotificationActionOptionForeground];

// Create the category with the custom actions.
UNNotificationCategory* expiredCategory = [UNNotificationCategory
      categoryWithIdentifier:@"TIMER_EXPIRED"
      actions:@[snoozeAction, stopAction]
      intentIdentifiers:@[]
      options:UNNotificationCategoryOptionNone];

// Register the notification categories.
UNUserNotificationCenter* center = [UNUserNotificationCenter currentNotificationCenter];
[center setNotificationCategories:[NSSet setWithObjects:generalCategory, expiredCategory, nil]];
```

UNTextInputNotificationAction允许用户在通知中输入

## Preparing Custom Alert Sounds

自定义的声音如果超过30秒，系统将播放默认的音频

afconvert音频转换工具

## Managing Your App’s Notification Settings

```
[UNUserNotificationCenter getNotificationSettingsWithCompletionHandler:];
```

## Managing Delivered Notifications

```
[UNUserNotificationCenter getDeliveredNotificationsWithCompletionHandler:];
[UNUserNotificationCenter removeDeliveredNotificationsWithIdentifiers:];
```



