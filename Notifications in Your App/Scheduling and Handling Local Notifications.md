# Scheduling and Handling Local Notifications

通知到达时，如果app处于前台，则app处理通知

## Configuring a Local Notification

创建本地通知的步骤：

1. 创建[UNMutableNotificationContent](https://developer.apple.com/documentation/usernotifications/unmutablenotificationcontent)
2. 创建[`UNCalendarNotificationTrigger`](https://developer.apple.com/documentation/usernotifications/uncalendarnotificationtrigger)、[`UNTimeIntervalNotificationTrigger`](https://developer.apple.com/documentation/usernotifications/untimeintervalnotificationtrigger)或者 [`UNLocationNotificationTrigger`](https://developer.apple.com/documentation/usernotifications/unlocationnotificationtrigger)
3. 用content和trigger创建[`UNNotificationRequest`](https://developer.apple.com/documentation/usernotifications/unnotificationrequest)

```
UNMutableNotificationContent* content = [[UNMutableNotificationContent alloc] init];
content.title = [NSString localizedUserNotificationStringForKey:@"Wake up!" arguments:nil];
content.body = [NSString localizedUserNotificationStringForKey:@"Rise and shine! It's morning time!" arguments:nil];

// Configure the trigger for a 7am wakeup.
NSDateComponents* date = [[NSDateComponents alloc] init];
date.hour = 7;
date.minute = 0;
UNCalendarNotificationTrigger* trigger = [UNCalendarNotificationTrigger triggerWithDateMatchingComponents:date repeats:NO];

// Create the request object.
UNNotificationRequest* request = [UNNotificationRequest requestWithIdentifier:@"MorningAlarm" content:content trigger:trigger];
```

#### Assigning Custom Actions to a Local Notification

```
UNNotificationContent *content = [[UNNotificationContent alloc] init];
// Configure the content. . .

// Assign the category (and the associated actions).
content.categoryIdentifier = @"TIMER_EXPIRED";

// Create the request and schedule the notification.
```

#### Adding a Sound to the Notification Content

```
content.sound = [UNNotificationSound soundNamed:@"MySound.aiff"];
```

## Responding to the Delivery of Notifications

为了响应通知，必须实现`UNUserNotificationCenter`的代理方法[UNUserNotificationCenterDelegate](https://developer.apple.com/documentation/usernotifications/unusernotificationcenterdelegate)

#### Handling Notifications When Your App Is in the Foreground

app在前台时，默认收到通知无声音。如果想做额外的处理，可以实现`UNUserNotificationCenter`的代理的[userNotificationCenter:willPresentNotification:withCompletionHandler:](https://developer.apple.com/documentation/usernotifications/unusernotificationcenterdelegate/1649518-usernotificationcenter)

```
- (void)userNotificationCenter:(UNUserNotificationCenter *)center 
       willPresentNotification:(UNNotification *)notification
         withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler {
   // Update the app interface directly.

    // Play a sound.
   completionHandler(UNNotificationPresentationOptionSound);
}
```

#### Responding to the Selection of a Custom Action

当用户点击自定义动作后，系统会通知app。`UNUserNotificationCenter`的代理的[userNotificationCenter:didReceiveNotificationResponse:withCompletionHandler:](https://developer.apple.com/documentation/usernotifications/unusernotificationcenterdelegate/1649501-usernotificationcenter)会被调用

如果app没在运行，系统会在后台启动app来处理自定义事件

Listing 3-5 Handling a custom notification action

```
- (void)userNotificationCenter:(UNUserNotificationCenter *)center
           didReceiveNotificationResponse:(UNNotificationResponse *)response
           withCompletionHandler:(void (^)(void))completionHandler {
    if ([response.notification.request.content.categoryIdentifier isEqualToString:@"TIMER_EXPIRED"]) {
        // Handle the actions for the expired timer.
        if ([response.actionIdentifier isEqualToString:@"SNOOZE_ACTION"]) {
            // Invalidate the old timer and create a new one. . .
        } else if ([response.actionIdentifier isEqualToString:@"STOP_ACTION"]) {
            // Invalidate the timer. . .
        }
    }
    // Else handle actions for other notification types. . .
}
```

#### Handling the Standard System Actions

系统动作被触发时，[`userNotificationCenter:didReceiveNotificationResponse:withCompletionHandler:`](https://developer.apple.com/documentation/usernotifications/unusernotificationcenterdelegate/1649501-usernotificationcenter)会被调用，response会包含以下action identifiers中的一种：

* [UNNotificationDismissActionIdentifier](https://developer.apple.com/documentation/usernotifications/unnotificationdismissactionidentifier)用户取消了通知，没选择自定义动作

* [UNNotificationDefaultActionIdentifier](https://developer.apple.com/documentation/usernotifications/unnotificationdefaultactionidentifier)用户打来了app，没选择自定义动作

Listing 3-6Handling the standard system actions

```
- (void)userNotificationCenter:(UNUserNotificationCenter *)center
          didReceiveNotificationResponse:(UNNotificationResponse *)response
          withCompletionHandler:(void (^)(void))completionHandler {
   if ([response.actionIdentifier isEqualToString:UNNotificationDismissActionIdentifier]) {
       // The user dismissed the notification without taking action.
   } else if ([response.actionIdentifier isEqualToString:UNNotificationDefaultActionIdentifier]) {
       // The user launched the app.
   }
   // Else handle any custom actions. . .
}
```



