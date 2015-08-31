# FrenchToast

*Stale Android Toasts made tasty.*

Android Toasts are amazing, but they have two major drawbacks:

* You cannot control **when they show up** as well as their **duration**. Other apps can enqueue toasts that will delay yours from showing up. 
* They [break context](http://cyrilmottier.com/2012/07/24/the-making-of-prixing-4-activity-tied-notifications/): they remain on screen when the user switches to other activities.

FrenchToast gives you absolute control over your app Toasts. It does so by duplicating the internals of Android Toasts and giving you access.

Unlike other *Toast-like* libraries, FrenchToast doesn't add a view to the root of your activity. Instead, it creates a new Window for each Toast, exactly like the real Android Toasts.

## Getting Started

In your `build.gradle`:

```gradle
 dependencies {
   compile 'info.piwai.frenchtoast:frenchtoast:TODO'
 }
```

You need to setup `FrenchToast` in your `Application` class:

```java
public class ExampleApplication extends Application {

  @Override public void onCreate() {
    super.onCreate();
    FrenchToast.install(this);
  }
}
```

You are ready to Toast!

```java
FrenchToast.with(context).showText("I love Baguettes!");
```

A `FrenchToast`:

* **hides** when the Activity is **paused**,
* shows again when its resumed,
* has a default duration of `Toast.LENGTH_LONG`,
* **survives configuration changes**,
* is **queued**, so that only one Toast shows at once.

You can customize the default duration:

```java
FrenchToast.with(context).shortLength().showText(R.string.short_bread);

FrenchToast.with(context).longLength().showText(R.string.long_bread);

FrenchToast.with(context).length(3, SECONDS).showText(R.string.bespoke_bread);
```

The duration of a Toast resets when the activity is paused / resumed, to make sure the user had enough time to see the Toast.

## Bespoke Toasts

A Toast can be created from a layout:

```java
FrenchToast.with(context).showLayout(R.layout.toasted_baguette);
```

You can also dip an Android Toast:

```java
Toast toast = Toast.makeText(context, "BREAD ALL THE THINGS!", LENGTH_SHORT);
toast.setGravity(LEFT | TOP, 0, 0);
FrenchToast.with(context).showDipped(toast);
```

## Unplugging the Toaster

A Toast can be canceled:

```
Toasted toasted = FrenchToast.with(context).showText("I love Baguettes!");
// I'd rather show a Bagel.
toasted.cancel();
```

You can also clear all queued Toasts for a given Activity:

```java
FrenchToast.with(context).clear();
```

## Context vs Activity

`FrenchToast.with()` takes a Context, however it expects that `Context` to be an `Activity` or to wrap an `Activity`, because FrenchToast keeps one `ToastQueue` for each activity.

## Crafting your own Mixture

If you want more control over when to show / hide Toasts, you can directly use `Mixture`:

```java
Toast toast = Toast.makeText(context, "BREAD ALL THE THINGS!", LENGTH_SHORT);
Mixture mixture = Mixture.dip(toast);
// The Toast is shown forever, as long as the process lives:
mixture.show();
// Or until you call hide:
mixture.hide();
```

![logo.png](assets/logo.png)

## License

    Copyright 2015 Pierre-Yves Ricau

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
