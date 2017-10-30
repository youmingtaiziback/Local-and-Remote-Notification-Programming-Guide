# Scheduling and Handling Local Notifications

通知到达时，如果app处于前台，则app处理系统

## Configuring a Local Notification

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

当用户点击自定义动作后，系统会通知app。UNUserNotificationCenter的代理的[userNotificationCenter:didReceiveNotificationResponse:withCompletionHandler:](https://developer.apple.com/documentation/usernotifications/unusernotificationcenterdelegate/1649501-usernotificationcenter)会被调用

