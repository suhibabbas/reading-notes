# Notifications

If your app can perform an action that might be useful from another app, your app should be prepared to respond to action requests by specifying the appropriate intent filter in your activity.

## Add an Intent Filter

In order to properly define which intents your activity can handle, each intent filter you add should be as specific as possible in terms of the type of action and data the activity accepts.

**Action:** A string naming the action to perform. Usually one of the platform-defined values such as `ACTION_SEND` or `ACTION_VIEW`.
Specify this in your intent filter with the `<action>` element. The value you specify in this element must be the full string name for the action, instead of the API constant.

**Data:** A description of the data associated with the intent.
Specify this in your intent filter with the `<data>` element. Using one or more attributes in this element, you can specify just the MIME type, just a URI prefix, just a URI scheme, or a combination of these and others that indicate the data type accepted.

**Catagory:** Provides an additional way to characterize the activity handling the intent, usually related to the user gesture or location from which it's started. There are several different categories supported by the system, but most are rarely used. However, all implicit intents are defined with `CATEGORY_DEFAULT` by default.

## Handle the Intent in Your Activity

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    setContentView(R.layout.main);

    // Get the intent that started this activity
    Intent intent = getIntent();
    Uri data = intent.getData();

    // Figure out what to do based on the intent type
    if (intent.getType().indexOf("image/") != -1) {
        // Handle intents with image data ...
    } else if (intent.getType().equals("text/plain")) {
        // Handle intents with text ...
    }
}
```

## Return a Result

If you want to return a result to the activity that invoked yours

```java
// Create intent to deliver some kind of result data
Intent result = new Intent("com.example.RESULT_ACTION", Uri.parse("content://result_uri"));
setResult(Activity.RESULT_OK, result);
finish();
```

If you simply need to return an integer that indicates one of several result options

```java
setResult(RESULT_COLOR_RED);
finish();
```

## Intent types

- **Explicit intents** specify which application will satisfy the intent, by supplying either the target app's package name or a fully-qualified component class name. You'll typically use an explicit intent to start a component in your own app, because you know the class name of the activity or service you want to start. For example, you might start a new activity within your app in response to a user action, or start a service to download a file in the background.

- **Implicit intents** do not name a specific component, but instead declare a general action to perform, which allows a component from another app to handle it. For example, if you want to show the user a location on a map, you can use an implicit intent to request that another capable app show a specified location on a map.