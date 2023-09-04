# 7 - Broadcast Receivers

Broadcast receivers are methods of receiving system events. When an event occurs, the code in the broadcast receiver is executed. A list of some (up to API 30) of the broadcast events to receive can be found [here](https://developer.android.com/about/versions/11/reference/broadcast-intents-30).

Create a new java class, and have it `extend BroadcastReceiver`. Then implement the required methods.

Before moving onto the rest of the receiver code, it needs to be registered in the `AndroidManifest.xml` file to receive actions. This is done using `action` tags in the `intent-filter` of the `receiver` as depicted in the ScribbleNotes [snippet](https://github.com/BA-Computer-Science-Club-2023-2024/ScribbleNotes/blob/ecd90c917646ecc35fdd56b54bac4b50e0740ac0/app/src/main/AndroidManifest.xml#L34-L39) below:
```xml
<receiver android:name=".BootReceiver"
  android:exported="false">
  <intent-filter>
    <action android:name="android.intent.action.BOOT_COMPLETED" />
  </intent-filter>
</receiver>
```

In the example above, the class inheriting from `BroadcastReceiver` is specified in `android:name`. The actions are defined in `android:name` of the `action` tag.

Next, returning to the code, one more if-statement is all that is needed before adding your desired code to the receiver.

```java
if (intent.getAction() != null && intent.getAction().equals(Intent.ACTION_BOOT_COMPLETED)) {
  // Your code
}
```
Surrounding your code with the snippet above ensures that the receiver is only called at the correct action.

An example of a use of a `BroadcastReceiver` is in ScribbleNotes, where the notification service made in the previous lesson is executed after a boot, to always allow the user to open the app:

The ScribbleNotes' [snippet](https://github.com/BA-Computer-Science-Club-2023-2024/ScribbleNotes/blob/ecd90c917646ecc35fdd56b54bac4b50e0740ac0/app/src/main/java/com/veryrandomcreator/scribblenotes/BootReceiver.java#L13-L29):
```java
public class BootReceiver extends BroadcastReceiver {
    /**
     * The default method used to respond to BroadcastReceiver actions.
     * This method checks for the correct event {@link Intent#ACTION_BOOT_COMPLETED}, and starts {@link NotificationService}
     *
     * @param context The Context in which the receiver is running.
     * @param intent The Intent being received.
     */
    @Override
    public void onReceive(Context context, Intent intent) {
        if (intent.getAction() != null && intent.getAction().equals(Intent.ACTION_BOOT_COMPLETED)) { // Checks for boot completed event
            Intent serviceIntent = new Intent(context, NotificationService.class); // Creates intent to launch service
            serviceIntent.setAction(NotificationService.START_SERVICE_ACTION); // Sets the action used by NotificationService, allowing it to bypass the toggle notification functionality
            context.startForegroundService(serviceIntent); // Starts the service
        }
    }
}
```
