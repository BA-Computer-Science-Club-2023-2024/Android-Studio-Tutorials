# 6a - Services and Notifications - Permissions

A service is a method of running non-blocking code in the background, even when the app is closed. 
There are multiple different types of services, however this tutorial series and the ScribbleNotes example app will only be using foreground services. More information on services in general, and the other types of services can be found [here](https://developer.android.com/guide/components/services).

## A Foreground Service

A foreground service is a type of service that runs while a notification is active. Some uses of a foreground service may be to run a notification to tell the user that the service is running while uploading, downloading, or updating. The example use of a foreground service in ScribbleNotes, is running an always active notification to allow the user to open the app anytime.

There are multiple parts involved in creating a foreground service, therefore the process is split into 2 parts, the first being the permissions needed for the service, the second being the service itself.

A screenshot of ScribbleNote's notification:
<img src="https://github.com/VeryRandomCreator/Computer-Science-Club-2023-2024/blob/main/images/6-ScribbleNotesNotification.png">

The notification can be expanded to show a button which opens the app:
<img src="https://github.com/VeryRandomCreator/Computer-Science-Club-2023-2024/blob/main/images/6-ScribbleNotesNotificationExpanded.png">

## Notification Permission

In order to post notifications, the app must have the necessary permissions from the user. Since the foreground service cannot run without notifications, it is important to check for the needed permissions and request them if necessary.

Permission can be requested from the user anytime, however it is ideal to request them at the time the action is supposed to occur (ex. launch notification button click) when possible. This makes the permission request more reasonable for the user, instead of getting a request without context. For more information into requesting permissions and the context behind requests, visit the [android website](https://developer.android.com/training/permissions/requesting.html).

For simplicity ScribbleNotes does not have a permission request rational prompt, and simply launches the request launcher when the service button is clicked.

### Check for permission

`ContextCompat.checkSelfPermission(CONTEXT, PERMISSION)` can check if the app has a specified permission.

The following is an example code [snippet](https://github.com/BA-Computer-Science-Club-2023-2024/ScribbleNotes/blob/607cc85772bd2754917c9487c491d39b6dab9326/app/src/main/java/com/veryrandomcreator/scribblenotes/MainActivity.java#L110-L125) from ScribbleNotes.
```java
/**
  * Checks whether the necessary permissions ({@link android.Manifest.permission#POST_NOTIFICATIONS}) are granted to the app
  *
  * @return A boolean of whether the permissions were granted
  */
public boolean hasPermissions() {
  if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.TIRAMISU) { // Ensures API is at level required to access the POST_NOTIFICATIONS permission check
    if (ContextCompat.checkSelfPermission(this, Manifest.permission.POST_NOTIFICATIONS) != PackageManager.PERMISSION_GRANTED) { // Checks if permission has been granted
      requestPermissionLauncher.launch(Manifest.permission.POST_NOTIFICATIONS); // If the permission has not been granted, request permission launcher is launched
      return false; // Does not currently have permission
    }
    return true; //Does have permission
  }
  return true;
}
```

The `hasPermissions()` method above uses `ContextCompat.checkSelfPermission(this, Manifest.permission.POST_NOTIFICATIONS)` to get the current permission state of the `Manifest.permission.POST_NOTIFICATIONS` permission. It then checks if it is granted, by comparing its equality to `PackageManager.PERMISSION_GRANTED`. If isn't, it requests the notification (next section), otherwise continues.

### Request permission

If permission is not already granted, the app needs to request the permission. A permission request can be made using an `ActivityResultLauncher<>` with a `ActivityResultContracts.RequestPermission()` contract. An `ActivityResultLauncher` is a resource to launch a non-blocking activity and handle the result. An `ActivityResultLauncher` can only be declared in an initialization method using `registerForActivityResult()`, such as `onCreate`, therefore a variable is needed to store the launcher after it is initialized, to be called in other code.

The following is a modified code snippet from the ScribbleNotes' [MainActivity.java](https://github.com/BA-Computer-Science-Club-2023-2024/ScribbleNotes/blob/main/app/src/main/java/com/veryrandomcreator/scribblenotes/MainActivity.java)
```java
/* import statements*/

/**
 * The main menu. Contains the title, notes list, create note button, and toggle notification button.
 */
public class MainActivity extends AppCompatActivity {
    /**
     * The {@link ActivityResultLauncher} to launch the permission request
     */
    private ActivityResultLauncher<String> requestPermissionLauncher;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        /* Field initalization & click listeners */

        requestPermissionLauncher = registerForActivityResult(new ActivityResultContracts.RequestPermission(), new ActivityResultCallback<Boolean>() { // Registers the permission request launcher, to be called to create the request dialog
            @Override
            public void onActivityResult(Boolean isAllowed) {
                if (!isAllowed) { // Checks if the permission is granted
                    sendToast("Notifications permission missing", getApplicationContext()); // If the permission is not granted, the user is notified accordingly
                } else {
                    sendToast("Permission granted", getApplicationContext()); // In the permission is granted, the user is notified accordingly
                }
            }
        });
    }

/* Rest of class*/
```

As depicted in the example, a field is declared to store the `ActivityResultLauncher`. `requestPermissionLauncher` is set using `registerForActivityResult(param1, param2)`, which can only be used during activity initialization.

#### Parameters of `registerForActivityResult(param1, param2)`:
1. ActivityResultContract<I, O> contract
   A contract relating to the type of activity to be launched. In this example case, a permission needs to be requested, therefore an instance of `ActivityResultContracts.RequestPermission()` is used.
2. ActivityResultCallback<O> callback
   An `ActivityResultCallback<O>` callback storing the code to be run once a result is received. `O` being the result type

Finally, the launcher can be launched using `requestPermissionLauncher.launch(Manifest.permission.POST_NOTIFICATIONS)`, in the case of the notifications permission in the ScribbleNotes code snippet.

Visit 6b - [Services and Notifications.md](../main/6b%20-%20Services%20and%20Notifications.md) for part 2 of running a foreground service.
