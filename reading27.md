# Intents, Activities, and SharedPreferences

## SharedPreferences

To save small collection of key-values, should use the `SharedPreferences` APIs. Each `SharedPreferences` file is managed by the framework and can be private or shared.

### Get a handle to shared preferences

To create a new shared preference file or access an existing one must use one of these methods:

1. `getSharedPreferences()`
2. `getPreferences()`

**example:**

```java
Context context = getActivity();
SharedPreferences sharedPref = context.getSharedPreferences(
        getString(R.string.preference_file_key), Context.MODE_PRIVATE);
```

```java
SharedPreferences sharedPref = getActivity().getPreferences(Context.MODE_PRIVATE);
```

### Write to shared preferences

To write to a shared preferences file, create a `SharedPreferences.Editor` by calling `edit()` on your `SharedPreferences`.

Pass the keys and values you want to write with methods such as `putInt()` and `putString()`. Then call `apply()` or `commit()` to save the changes. For example:

```java
SharedPreferences sharedPref = getActivity().getPreferences(Context.MODE_PRIVATE);
SharedPreferences.Editor editor = sharedPref.edit();
editor.putInt(getString(R.string.saved_high_score_key), newHighScore);
editor.apply();
```

### Read from shared preferences

To retrieve values from a shared preferences file, call methods such as `getInt()` and `getString()`, providing the key for the value you want, and optionally a default value to return if the key isn't present. For example:

```java
SharedPreferences sharedPref = getActivity().getPreferences(Context.MODE_PRIVATE);
int defaultValue = getResources().getInteger(R.integer.saved_high_score_default_key);
int highScore = sharedPref.getInt(getString(R.string.saved_high_score_key), defaultValue);
```

## Espresso

```Java
@Test
public void greeterSaysHello() {
    onView(withId(R.id.name_field)).perform(typeText("Steve"));
    onView(withId(R.id.greet_button)).perform(click());
    onView(withText("Hello Steve!")).check(matches(isDisplayed()));
}
```

Espresso tests run optimally fast! It lets you leave your waits, syncs, sleeps, and polls behind while it manipulates and asserts on the application UI when it is at rest.

### Target audience

Espresso is targeted at developers, who believe that automated testing is an integral part of the development lifecycle. While it can be used for black-box testing, Espresso’s full power is unlocked by those who are familiar with the codebase under test.

### Synchronization capabilities

Each time your test invokes onView(), Espresso waits to perform the corresponding UI action or assertion until the following synchronization conditions are met:

- The message queue is empty.
- There are no instances of AsyncTask currently executing a task.
- All developer-defined idling resources are idle.

### Packages

- `espresso-core` - Contains core and basic `View` matchers, actions, and assertions.
- `espresso-web` - Contains resources for `WebView` support.
- `espresso-idling-resource` - Espresso's mechanism for synchronization with background jobs.
- `espresso-contrib` - External contributions that contain `DatePicker`, `RecyclerView` and `Drawer` actions, accessibility checks, and `CountingIdlingResource`.
- `espresso-intents` - Extension to validate and stub intents for hermetic testing.
- `espresso-remote` - Location of Espresso's multi-process functionality.
