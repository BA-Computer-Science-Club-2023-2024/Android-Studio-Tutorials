# 5 - Fragments

## Set up

Fragments are lightweight components, with functionality similar to that of an activity, and much more. ScribbleNotes uses a fragment simply as a lightweight replacement of an activity, however it has the potential for more versatile applications (such as dialogs). 

A fragment can be created from a variety of templates, by right-clicking bottom package of `src` -> `New` -> `Fragment` -> Selecting an item on the list of fragment templates.

Navigation to fragment templates image:
<img src="https://github.com/VeryRandomCreator/Computer-Science-Club-2023-2024/blob/main/images/5-CreateFragment.png">

Name the fragment in the next window, then click `Finish`:
<img src="https://github.com/VeryRandomCreator/Computer-Science-Club-2023-2024/blob/main/images/5-NameFragment.png">

The newly created fragment class will contain a lot of code unnecessary to the purposes of this example, so replace the code with the following (replacing `YOUR.PACKAGE.NAME` with your package, and `FRAGMENT_LAYOUT_ID` with the layout resource name of the fragment (found in `res` -> `layout`)):

```java
package YOUR.PACKAGE.NAME;

import android.os.Bundle;

import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.fragment.app.Fragment;

import android.view.View;

public class ExampleFragment extends Fragment {
    public ExampleFragment() {
        super(R.layout.FRAGMENT_LAYOUT_ID); // Calls the parent's constructor to set the layout of this fragment
    }

    @Override
    public void onViewCreated(@NonNull View view, @Nullable Bundle savedInstanceState) {
        super.onViewCreated(view, savedInstanceState);
    }
}
```

`super(R.layout.FRAGMENT_LAYOUT_ID);` is vital, by linking the layout resource to the fragment's code.

## Entering and exiting

### Entering

Entering fragments can be managed through creating transactions in `getSupportFragmentManager()`.

The following is a code [snippet](https://github.com/BA-Computer-Science-Club-2023-2024/ScribbleNotes/blob/c4f1993f89465822ca455947494080bead5cfcf0/app/src/main/java/com/veryrandomcreator/scribblenotes/MainActivity.java#L135-L143) from ScribbleNotes:
```java
/**
  * Creates the editor fragment from supplied position in list/storage
  * <p>
  * This code follows the code examples found on the <a href="https://developer.android.com/guide/fragments/create">Android Developer Website</a>,
  * which contains more information on creating Fragments
  *
  * @param position The position of the file on the clicked list
  */
public void openEditorFragment(int position) {
  Bundle bundle = new Bundle(); // A bundle to pass data to fragment
  bundle.putInt("position", position); // Storing the position of the file to load into a note in a bundle to give to fragment

  getSupportFragmentManager().beginTransaction()
    .setReorderingAllowed(true)
    .add(R.id.fragmentContainerView, EditorFragment.class, bundle)
    .commit(); // Launches the fragment
  }
```

`getSupportFragmentManager().beginTransaction()...add(param1, param2, param3)` creates the create fragment transaction. Adding more chained methods can further customize it.

`getSupportFragmentManager().beginTransaction().add(param1, param2, param3)` parameters:

 - `param1`: View to replace with fragment.
   A FragmentContainerView can be used as a temporary view that will be replaced with the fragment.

   Put the view on the very top level of the layout, for the fragment to overlay over all the components when launched.

   An example of the FragmentContainerView usage in ScribbleNotes:
   <img src="https://github.com/VeryRandomCreator/Computer-Science-Club-2023-2024/blob/main/images/5-FragmentContainerView.png">
 - `param2`: Fragment class. 
   Ex: `EditorFragment.class`
 - `param3`: Bundle to pass to fragment
   This parameter stores the bundle that can be retrieved in the fragment through `requireArguments()`.

  This is used to pass arguments to the fragment making it unique and versatile among different cases.
  Ex: 
  ```java
  Bundle bundle = new Bundle(); // A bundle to pass data to fragment
  bundle.putInt("position", position); // Storing the position of the file to load into a note in a bundle to give to fragment
  ```

  This code is used in ScribbleNotes to allow the fragment to load the data for a unique file to the fields. 

### Exiting

Exiting a fragment is also done through transactions in `getSupportFragmentManager().beginTransaction()`.

The fragment can close itself using `requireActivity().getSupportFragmentManager().beginTransaction().remove(this).commit();`

`requireActivity()` retrieves the parent activity of the fragment, then uses its fragment manager like the process of making it, except uses `remove(FRAGMENT)` instead of `add()`.

A close fragment method like in [ScribbleNotes](https://github.com/BA-Computer-Science-Club-2023-2024/ScribbleNotes/blob/c4f1993f89465822ca455947494080bead5cfcf0/app/src/main/java/com/veryrandomcreator/scribblenotes/EditorFragment.java#L205-L219)' can make it easier to close fragments:
```java
/**
  * Exits the editor activity. Sets all layout references to null and updates {@link MainActivity}
  */
public void closeFragment() {
  noteEdtTxt = null;
  titleEdtTxt = null;
  titleCard = null;
  cancelBtn = null;
  deleteBtn = null;
  saveBtn = null;
  optionsBtn = null;
  requireActivity().getSupportFragmentManager().beginTransaction().remove(this).commit(); // Exits fragment
}
```
