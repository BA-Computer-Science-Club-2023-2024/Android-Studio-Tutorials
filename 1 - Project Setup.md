# 1 - Project Setup

In the Android Studio IDE, navigate to the `New Project` button.

Next, select the `Empty Views Activity` project template in the window shown below.

<img src="https://github.com/VeryRandomCreator/Computer-Science-Club-2023-2024/blob/main/images/1-Project_Template.PNG">

The next window will contain text fields for the project `Name`, `Package name`, `Save location`, `Language`, `Minimum SDK`, and `Build configuration language`

1. <h4>Name</h4>  

   This field contains the name of the project. This will be the default app name generated for your app.
   
   Ex. `Name:` `ScribbleNotes`
   
3. <h4>Package name</h4>

   The name of the package containing the project files. Can only contain numbers, letters, and periods when seperating packages. (No special characters). Common convention for the package to be written in all lowercase
   
   Ex. `Package name:` `com.veryrandomcreator.scribblenotes`
   
5. <h4>Save location</h4>
   The directory to save the project.

   Ex. `Directory name:` `C:\Users\VeryRandomCreator\Desktop\Code\AndroidStudio\ScribbleNotes
   `
7. <h4>Language</h4>

   The language of the project. (`Java`, or `Kotlin`)

   Ex. `Language:` `Java`
   
9.  <h4>Minimum SDK</h4>

    The minimum SDK version of the project. This affects what classes and methods you can access. Ex. You cannot use a method for API 26 if your minimum SDK is 27.

    *Then why wouldn't you set the minimum to the lowest level to get access to all deprecated methods?* Setting the minimum to the lowest available level may lead to problems in which devices run on a API level lower than the level required to call a method, making certian features unavailable to certain devices.

    Ideally, choose a minimum version that targets the most devices, while allowing them to access all the app features. `ScribbleNotes` with a minimum level of `API 27` allows all devices >= `API 27` to access all the features of the app.

    Android studio provides a helpful information message to understand the selected version's possible reach:

    <img src="https://github.com/VeryRandomCreator/Computer-Science-Club-2023-2024/blob/main/images/1-MinimumSDKDialog.PNG">

    Ex: `Minimum SDK:` `API 27 ("Oreo"; Android 8.1)`

11. <h4>Build configuration language</h4>

    The language of the build config file. (`Kotlin DSL (build.gradle.kts)` or `Groovy DSL (build.gradle)`)

    The `build.gradle` or `build.gradle.kts` file contains the app's config information including `applicationId`, `minSdk`, `targetSdk`, and more, which can be edited in `File -> Project Structure -> Modules -> Default Config`. The file is also where you edit and import dependencies.
