# 6b - Services and Notifications - Building Service

Once all the necessary permissions are managed, you can begin creating the foreground service. 

## Creating the service

Use Android Studio to generate a service file by right-clicking bottom package of `src` -> `New` -> `Fragment` -> and Selecting `Service` as shown below

<img src="https://github.com/VeryRandomCreator/Computer-Science-Club-2023-2024/blob/main/images/6b-CreateService.png">

In the next window, name the class, and check `Enabled` and uncheck `Exported`. `Enabled` refers to the service's ability to be instantiated, in case you want to control the service's use. `Exported` refers to whether other applications can access the service.

For this tutorial's use, `onBind` is not used, therefore it can return null, like the following:
```java
@Override
public IBinder onBind(Intent intent) {
  return null;
}
```

`onStartCommand` is called when the service is called, therefore the functionality is built on that event:
```java
@Override
public int onStartCommand(Intent intent, int flags, int startId) {
  // Code goes here
  return super.onStartCommand(intent, flags, startId);
}
```

## Creating the notification

Since a notification is needed for the service, the next step is creating the notification.

### Notification channel

To create the notification needed for the service, `NotificationManagerCompat` is used to create a `NotificationChannel`. A notification channel allows notifications to be grouped and for the user to perform general actions (like muting) on these groups of notifications.

A [code snippet](https://github.com/BA-Computer-Science-Club-2023-2024/ScribbleNotes/blob/ecd90c917646ecc35fdd56b54bac4b50e0740ac0/app/src/main/java/com/veryrandomcreator/scribblenotes/NotificationService.java#L79-L80) from ScribbleNotes:
```java
NotificationManagerCompat notificationManager = NotificationManagerCompat.from(this); // The notification manager to manage posting notifications
notificationManager.createNotificationChannel(new NotificationChannel(NOTIFICATION_CHANNEL_ID, "Scribble Notes Notification Channel", NotificationManager.IMPORTANCE_LOW)); // The channel for the notification
```

### Notification content

Next is building the notification itself. Notifications are built using a series of chained `NotificationCompat.Builder` calls.

A [code snippet](https://github.com/BA-Computer-Science-Club-2023-2024/ScribbleNotes/blob/ecd90c917646ecc35fdd56b54bac4b50e0740ac0/app/src/main/java/com/veryrandomcreator/scribblenotes/NotificationService.java#L83-L88) from ScribbleNotes:
```java
NotificationCompat.Builder serverNotificationBuilder = new NotificationCompat.Builder(this, NOTIFICATION_CHANNEL_ID) // Sets the channel for the notification
  .setSmallIcon(R.drawable.ic_notifications_on) // Sets the notification icon
  .setContentTitle("Create new note") // Sets the notification title
  .setContentText("Click to create new note") // Sets the notification content text
  .setPriority(NotificationCompat.PRIORITY_DEFAULT) // Sets the priority of the notification
  .setOngoing(true) // Makes the notification unable to be swiped away
```

The comments beside each line indicate their purpose.

### Notification action

To add an action, make a `.addAction()` call with a `NotificationCompat.Action` instance.

The [example](https://github.com/BA-Computer-Science-Club-2023-2024/ScribbleNotes/blob/ecd90c917646ecc35fdd56b54bac4b50e0740ac0/app/src/main/java/com/veryrandomcreator/scribblenotes/NotificationService.java#L89-L92) in ScribbleNotes:
```java
.addAction(new NotificationCompat.Action(R.drawable.ic_notifications_on, // Sets action's icon
  "Create Note", // Sets the action's text
  PendingIntent.getActivity(this, NOTIFICATION_ACTION_REQUEST_CODE, startActivityIntent, PendingIntent.FLAG_UPDATE_CURRENT | PendingIntent.FLAG_MUTABLE)) // Sets the action
);
```

The `startActivityIntent` variable being whatever action intent chosen. In the case of ScribbleNotes, it launches the app:
```java
Intent startActivityIntent = new Intent(this, MainActivity.class); // The intent to start the app
```

### Building everything

Combining all the parts of creating a notification through `.build()` and a `notificationManager.notify()` call creates and pushes the notification to the user. Then a final call of `startForeground()` links the notification pushed to the user to the current service.

The following is the entire ScribbleNotes [example](https://github.com/BA-Computer-Science-Club-2023-2024/ScribbleNotes/blob/ecd90c917646ecc35fdd56b54bac4b50e0740ac0/app/src/main/java/com/veryrandomcreator/scribblenotes/NotificationService.java#L79-L98):
```java
NotificationManagerCompat notificationManager = NotificationManagerCompat.from(this); // The notification manager to manage posting notifications
notificationManager.createNotificationChannel(new NotificationChannel(NOTIFICATION_CHANNEL_ID, "Scribble Notes Notification Channel", NotificationManager.IMPORTANCE_LOW)); // The channel for the notification

Intent startActivityIntent = new Intent(this, MainActivity.class); // The intent to start the app
NotificationCompat.Builder serverNotificationBuilder = new NotificationCompat.Builder(this, NOTIFICATION_CHANNEL_ID) // Sets the channel for the notification
  .setSmallIcon(R.drawable.ic_notifications_on) // Sets the notification icon
  .setContentTitle("Create new note") // Sets the notification title
  .setContentText("Click to create new note") // Sets the notification content text
  .setPriority(NotificationCompat.PRIORITY_DEFAULT) // Sets the priority of the notification
  .setOngoing(true) // Makes the notification unable to be swiped away
  .addAction(new NotificationCompat.Action(R.drawable.ic_notifications_on, // Sets action's icon
    "Create Note", // Sets the action's text
    PendingIntent.getActivity(this, NOTIFICATION_ACTION_REQUEST_CODE, startActivityIntent, PendingIntent.FLAG_UPDATE_CURRENT | PendingIntent.FLAG_MUTABLE)) // Sets the action
    );
Notification notification = serverNotificationBuilder.build(); // Builds the notification
if (ContextCompat.checkSelfPermission(this, Manifest.permission.POST_NOTIFICATIONS) == PackageManager.PERMISSION_GRANTED) { // Checks the notification permission
  notificationManager.notify(NOTIFICATION_ID, notification); // Sends the notification
  startForeground(NOTIFICATION_ID, notification); // Links the pushed notification to the service to allow it to continue running as a foreground service
}
```
