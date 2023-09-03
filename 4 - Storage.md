# Storage

Storing data is an essential part of creating apps.
Manipulating files for android apps is just as easy as manipulating files in ordinary java, without any third-party libraries.
Simply storing a file reference in a `java.io.File`.

## Writing

Below is a modified [code snippet](https://github.com/BA-Computer-Science-Club-2023-2024/ScribbleNotes/blob/c4f1993f89465822ca455947494080bead5cfcf0/app/src/main/java/com/veryrandomcreator/scribblenotes/Note.java#L130C1-L151) from the ScribbleNotes example project. `fileName`, `title`, `content` and `DELIMITER` are predefined strings, storing their the information corresponding to their name. (`fileName` is the name of the file including extension to save as in storage, `title` and `content` relate to the specific data being stored in the file in this example case, which is the title and content of the note, and `DELIMITER` acts as a seperator between the content stored in the note.)

```java
/**
 * Saves data stored in fields to file.
 *
 * @param context The {@link Context} needed to access {@link Context#getFilesDir()}
 */
public void saveToFile(Context context) {
  try {
    File saveFile = new File(context.getFilesDir(), fileName); // Creates a File instance of a file in the storage directory under the file name
    saveFile.createNewFile(); // Creates a new file (if necessary)

    FileWriter noteFileWriter = new FileWriter(saveFile); // Creates a new instance of FileWriter to write to the file
    noteFileWriter.append(title); // Writes the title to the file
    noteFileWriter.append(DELIMITER); // Writes the delimiter to the file to split the title and the rest of the content
    noteFileWriter.append(content); // Writes te content to the file
    noteFileWriter.close(); // Closes the FileWriter
  } catch (IOException e) {
    e.printStackTrace();
  }
}
```

A reference to the file can be made using `new File(context.getFilesDir(), fileName)`, the first parameter being the storage directory, which is retrieved with `context.getFilesDir()`, and the second parameter being the name of the file. 

All other file operations (creating the new file with `File#createNewFile()`, writing to file with stream) do not differ from ordinary java file manipulation.

ScribbleNotes sticks to simple file writing using `java.io.FileWriter`, as shown in the code snippet.

## Reading

Finding and reading from a file by name can be done by iterating through the list of files in storage (`context.getFilesDir().listFiles()`) and checking for the requested file name. Then ordinary `java.io.File` reading techniques can be applied. 

The example below contains modified [code](https://github.com/BA-Computer-Science-Club-2023-2024/ScribbleNotes/blob/c4f1993f89465822ca455947494080bead5cfcf0/app/src/main/java/com/veryrandomcreator/scribblenotes/Note.java#L106C9-L127C10) from ScribbleNotes. This example uses `java.nio.file.Files.readAllBytes()` to read the contents of the file to a string. Although it may not be an efficient or performance-safe file-reading method, it was chosen for simplicity while teaching.

```java
File[] storageFiles = context.getFilesDir().listFiles(); // Retrieves the list of files stored in the app's directory
if (storageFiles == null) { // Null check to remove Android Studio's warning about null-safety
  System.err.println("Error while loading storage");
  return;
}
for (File file : storageFiles) { // Iterates through the files
  if (file.getName().equals(fileName)) { // Checks if the file is the requested name
    try {
      String fileContent = new String(Files.readAllBytes(file)); // Reads the file contents to a string. Not a very efficient and performance-safe way of reading file (in case the file is very large, but fulfills the need)
      /*
        Use file content
      */
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```

## Deleting

Android has a simple method of deleting files, using `context.deleteFile(fileName)` as shown in ScribbleNotes [code snippet](https://github.com/BA-Computer-Science-Club-2023-2024/ScribbleNotes/blob/c4f1993f89465822ca455947494080bead5cfcf0/app/src/main/java/com/veryrandomcreator/scribblenotes/Note.java#L168-L173) below:

```java
public void deleteFile(Context context) {
  if (fileName != null) { // Checks whether the file name is null
    context.deleteFile(fileName); // If the file name is not null, deletes the file based on the file name
  }
}
```
