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
UNCalendarNotificationTrigger* trigger = [UNCalendarNotificationTrigger
       triggerWithDateMatchingComponents:date repeats:NO];

// Create the request object.
UNNotificationRequest* request = [UNNotificationRequest requestWithIdentifier:@"MorningAlarm" content:content trigger:trigger];
```



