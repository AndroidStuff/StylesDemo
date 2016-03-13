# StylesDemo

## About
The purpose of Styles demo project is to show case how the inline styles applied in the layout files can be exported to Styles.xml file

We also get to learn in the process as to how some styles (that are applicable on the parent of the view) cannot be exported. In the case of this example `android:layout_marginBottom` or any `android:layout_*` cannot be exported to the Styles.xml because layout/margin styles is applied on the container views or ViewGroups. Better explanation for this can be found at http://stackoverflow.com/a/13365288.

The same answer is reproduced below for my quick reference (atleast until I can explain it better in my own words :).



**Short Answer:** If you are specifying layout_margin in a custom style, this style must be explicitly applied to each individual view that you wish to have the specified margin (as seen in the code sample below). Including this style in a theme and applying it to your application or an activity will not work.

```html
<TableLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:background="#FFFFFF" >
    <TableRow>
        <EditText android:hint="@string/last_name" style="@style/edit_text_default" />
    </TableRow>
    <TableRow>
        <EditText android:hint="@string/first_name" style="@style/edit_text_default" />
    </TableRow>
</TableLayout>
```

**Explanation:** Attributes which begin with layout_ are [LayoutParams](http://developer.android.com/reference/android/view/ViewGroup.LayoutParams.html), or one of its subclasses (e.g. [MarginLayoutParams](http://developer.android.com/reference/android/view/ViewGroup.MarginLayoutParams.html)). LayoutParams are used by views to tell their parent [ViewGroup](http://developer.android.com/reference/android/view/ViewGroup.html) how they want to be laid out. Each and every ViewGroup class implements a nested class that extends ViewGroup.LayoutParams. Therefore, LayoutParams are specific to the ViewGroup's type. What this means is that while a TableLayout and a LinearLayout may both have layout_margin as one of it's LayoutParams, they are considered to be completely different attributes.

So layout_margin is not just general attribute that can be applied anywhere. It must be applied within the context of a ViewGroup that specifically defines it as a valid argument. A view must be aware of the type of its parent ViewGroup when LayoutParams are applied.

Specifying layout_margin in a style, including that style in a theme, and attempting to apply that theme to an application/activity will result in the layout attributes being dropped, because no ViewGroup parent has been specified yet and so the arguments are invalid. However, applying the style to an EditText view that has been defined with a TableLayout works, because the parent ViewGroup (the TableLayout) is known.

**Sources:**

Android documentation on [Layout Parameters](http://developer.android.com/guide/topics/ui/declaring-layout.html#layout-params).

Answer given to [this question](http://stackoverflow.com/questions/5315529/layout-problem-with-button-margin) by Android framework engineer and StackOverflow user [adamp](http://stackoverflow.com/users/342605/adamp).

Also, answer given to [this question](http://stackoverflow.com/questions/6453877/setting-layout-parameters-in-android) by StackOverflow user [inazaruk](http://stackoverflow.com/users/69802/inazaruk).
