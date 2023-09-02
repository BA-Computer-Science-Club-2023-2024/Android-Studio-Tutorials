# 2 - Adding views

_What is a view?_ Views are interfaces for other views or the user to interact with. Buttons, text fields, lists, and many more are types of views.

To add a view, open the `.xml` layout file, of the layout you are trying to add a view to. Next, find the chosen view to add in the `Palette` of the `Design` tab in the xml layout editor, and drag it onto the layout:

<img src="https://github.com/VeryRandomCreator/Computer-Science-Club-2023-2024/blob/main/images/2-Drag_Views.PNG">

Next, adjust the view's constraints by pulling the nodes on the edges of the view, to the edges of other views.

<img src="https://github.com/VeryRandomCreator/Computer-Science-Club-2023-2024/blob/main/images/2-ConstraintViews.png" height=600>

The view's position can be adjusted in-code using position-related tags:

```xml
    <Space
        android:id="@+id/space"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.05"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
```

This is a snippet from the [ScribbleNotes](https://github.com/BA-Computer-Science-Club-2023-2024/ScribbleNotes) example project, specifically an invisible [space](https://github.com/BA-Computer-Science-Club-2023-2024/ScribbleNotes/blob/c4f1993f89465822ca455947494080bead5cfcf0/app/src/main/res/layout/note_list_item.xml#L22-L30) view used to align the TextView of the list items. 

The result of the `app:layout_constraintHorizontal_bias` tag makes the space horizontally aligned like so:

<img src="https://github.com/VeryRandomCreator/Computer-Science-Club-2023-2024/blob/main/images/2-XmlConstraints.PNG" height=500>

The `app:layout_constraintHorizontal_bias` tag takes a decimal as a parameter, representing the percent position in parent. Ex. `app:layout_constraintHorizontal_bias="0.05"` means alignment to 5% of parent width.

Move the view to the desired location. It is best practice to move views relative to other views, and with the `layout_constraintVertical_bias` and `layout_constraintHorizontal_bias` tags in xml code, rather than constant positions. This allows for the views to dynamically adjust on different screen sizes.

To get a good idea of the different types of tags, explore the `Attributes` tab on the right side of the design editor:

<img src="https://github.com/VeryRandomCreator/Computer-Science-Club-2023-2024/blob/main/images/2-AttributesTab.PNG" height=500>

The best way to learn the wide varity of tags and how to use the design editor is through trial and error. There are a vast number of tags, and parameters for the tags, making it almost impossible for someone to sit down and teach, therefore learn by creating dozens of layouts and apps yourself to pick up proper techniques and knowledge.
