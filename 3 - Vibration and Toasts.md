# 4 - Vibration and Toasts

## Vibration

Vibration is used in apps to indicate to users that certain actions have been completed, or events have been triggered.

To vibrate the device the vibrator service is needed:
`Vibrator vibrator = (Vibrator) getSystemService(Context.VIBRATOR_SERVICE);`

Next, simply call the `vibrate(VibrationEffect)` method to vibrate the device: 
`vibrator.vibrate(VibrationEffect.createOneShot(MILLISECONDS, AMPLITUDE));`

Replace `MILLISECONDS` and `AMPLITUDE` with the amount of time in milliseconds the vibration should last, and the amplitutde of the vibration.

The following is an example of a method which vibrates the device:

```java
/**
 * A method to vibrate the device
*/
public void vibrate() {
  Vibrator vibrator = (Vibrator) getSystemService(Context.VIBRATOR_SERVICE); // Retrieves the vibrator service
  vibrator.vibrate(VibrationEffect.createOneShot(100, VibrationEffect.DEFAULT_AMPLITUDE)); // Vibrates the device at the default amplitude for 100 milliseconds
}
```

## Toasts

Toasts are small popup bubbles that appear at the bottom of the screen to notify the user of something. 

<img src="https://github.com/VeryRandomCreator/Computer-Science-Club-2023-2024/blob/main/images/3-Toast.png" width=300>

To create a toast, call `Toast.makeText(CONTEXT, TEXT, DURATION)`.

Replace `CONTEXT` with the context of the current application or service, usually only using the `this` keyword. Replace `TEXT` with a string of the text to display in the toast. Replace `DURATION` with either `Toast.LENGTH_LONG` or `Toast.LENGTH_SHORT`.

The following is an example of a method to create a toast with the specified text:
```java
/**
* A method to send a toast
*
* @param text The text to display in the toast
*/
public void sendToast(String text) {
  Toast.makeText(this, text, Toast.LENGTH_LONG).show(); // Creates a toast with the specified text
}
```
