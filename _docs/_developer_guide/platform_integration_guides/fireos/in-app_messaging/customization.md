---
nav_title: Customization
platform: FireOS
page_order: 2
---

# Customization {#in-app-message-customization}
All of Braze’s in-app message types are highly customizable across messages, images, [Font Awesome][15]  icons, click actions, analytics, editable styling, custom display options, and custom delivery options. Multiple options can be configured on a per in-app message basis from [within the dashboard]({{site.baseurl}}/user_guide/message_building_by_channel/in-app_messages/create/). Braze additionally provides multiple levels of advanced customization to satisfy a variety of use cases and needs.

## Key Value Pair Extras

In-app message objects may carry key value pairs as `extras`. They are specified on the dashboard under "Advanced Settings" when creating an in-app message campaign. These can be used to send data down along with an in-app message for further handling by the application.

Call the following when you get an in-app message object to retrieve its extras:

```java
Map<String, String> getExtras()
```

See the [Javadoc][44] for more information.

## Custom Styling

Braze UI elements come with a default look and feel that matches the Android standard UI guidelines and provides a seamless experience. You can see these default styles in the Braze SDK's [`styles.xml`][6] file.

```xml
  <style name="Appboy"/>
    <!-- In-app Message -->
  <style name="Appboy.InAppMessage">
  </style>
  <style name="Appboy.InAppMessage.Header">
    <item name="android:layout_height">wrap_content</item>
    <item name="android:layout_width">wrap_content</item>
    <item name="android:padding">0.0dp</item>
    <item name="android:background">@android:color/transparent</item>
    <item name="android:textColor">@color/com_appboy_inappmessage_header_text_light</item>
    <item name="android:textSize">19.0sp</item>
    <item name="android:layout_gravity">center</item>
    <item name="android:singleLine">true</item>
    <item name="android:textStyle">bold</item>
  </style>
```

If you would prefer, you can override these styles to create a look and feel that better suits your app.

To override a style, copy it in its entirety to the `styles.xml` file in your own project and make modifications. The whole style must be copied over to your local `styles.xml` file in order for all attributes to be correctly set. Please note that these custom styles are for changes to individual UI elements, not wholesale changes to layouts. Layout-level changes need to be handled with custom views.

### Using Custom Styling to Set a Custom Font

Braze allows for setting a custom font using the [font family guide][79]. To use it, override the style for message text, headers, and/or button text and use the `fontFamily` attribute to instruct Braze to use your custom font family.

For example, to update the font on your in-app message button text, override the `Appboy.InAppMessage.Button` style and reference your custom font family. The attribute value should point to a font family in your `res/font` directory.

Here is a truncated example with a custom font family, `my_custom_font_family`, referenced on the last line:

```
<style name="Appboy.InAppMessage.Button">
  <item name="android:layout_height">wrap_content</item>
  ...
  <item name="android:paddingBottom">15.0dp</item>
  <item name="android:fontFamily">@font/my_custom_font_family</item>
  <item name="fontFamily">@font/my_custom_font_family</item>
</style>
```

Aside from the `Appboy.InAppMessage.Button` style for button text, the style for message text is `Appboy.InAppMessage.Message` and the style for message headers is `Appboy.InAppMessage.Header`. If you want to use your custom font family across all possible in-app message text, you can set your font family on the `Appboy.InAppMessage` style, which is the parent style for all in-app messages.

>  As with other custom styles, the entire style must be copied over to your local `styles.xml` file for all attributes to be correctly set.

## Setting Custom Listeners

Before customizing in-app messages with custom listeners, it's important to understand the [`AppboyInAppMessageManager`][34], which handles the majority of in-app message handling. As described in [Step 1][5], it must be registered for in-app messages to function appropriately.

`AppboyInAppMessageManager` manages in-app message display on Android.  It contains helper class instances that help it manage the lifecycle and display of in-app messages. All of these classes have standard implementations and defining custom classes is completely optional. However, doing so can add another level of control over the display and behavior of in-app messages.  These customizable classes include:

- [`IInAppMessageManagerListener`][21] - Implement to [custom manage in-app message display and behavior][19].
- [`IInAppMessageViewFactory`][42] - Implement to [build custom in-app message views][12].
- [`IInAppMessageAnimationFactory`][20] - Implement to [define custom in-app message animations][22].

### Setting a Custom Manager Listener

The `AppboyInAppMessageManager` automatically handles the display and lifecycle of in-app messages.  If you require more control over the lifecycle of a message, setting a custom manager listener will enable you to receive the in-app message object at various points in the in-app message lifecycle, allowing you to handle its display yourself, perform further processing, react to user behavior, process the object's [Extras][14], and much more.

#### Step 1: Implement an In-App Message Manager Listener

Create a class that implements [`IInAppMessageManagerListener`][21]

The callbacks in your `IInAppMessageManagerListener` will be called at various points in the in-app message lifecycle.

For example, if you set a custom manager listener, when an in-app message is received from Braze, the `beforeInAppMessageDisplayed()` method will be called. If your implementation of this method returns [`InAppMessageOperation.DISCARD`][83], that signals to Braze that the in-app message will be handled by the host app and should not be displayed by Braze. If `InAppMessageOperation.DISPLAY_NOW` is returned, Braze will attempt to display the in-app message. This method should be used if you choose to display the in-app message in a customized manner.

`IInAppMessageManagerListener` also includes delegate methods for clicks on the message itself or one of the buttons.  A common use case would be intercepting a message when a button or message is clicked for further processing.

{% tabs %}
{% tab JAVA %}

```java
public class CustomInAppMessageManagerListener implements IInAppMessageManagerListener {
  private final Activity mActivity;

  public CustomInAppMessageManagerListener(Activity activity) {
    mActivity = activity;
  }

  @Override
  public boolean onInAppMessageReceived(IInAppMessage inAppMessage) {
    return false;
  }

  @Override
  public InAppMessageOperation beforeInAppMessageDisplayed(IInAppMessage inAppMessage) {
    return InAppMessageOperation.DISPLAY_NOW;
  }

  @Override
  public boolean onInAppMessageClicked(IInAppMessage inAppMessage, InAppMessageCloser inAppMessageCloser) {
    Toast.makeText(mActivity, "The click was ignored.", Toast.LENGTH_LONG).show();

    // Closing should not be animated if transitioning to a new activity.
    // If remaining in the same activity, closing should be animated.
    inAppMessageCloser.close(true);
    return true;
  }

  @Override
  public boolean onInAppMessageButtonClicked(IInAppMessage inAppMessage, MessageButton button, InAppMessageCloser inAppMessageCloser) {
    Toast.makeText(mActivity, "The button click was ignored.", Toast.LENGTH_LONG).show();

    // Closing should not be animated if transitioning to a new activity.
    // If remaining in the same activity, closing should be animated.
    inAppMessageCloser.close(true);
    return true;
  }

  @Override
  public void onInAppMessageDismissed(IInAppMessage inAppMessage) {
    if (inAppMessage.getExtras() != null && !inAppMessage.getExtras().isEmpty()) {
      Map<String, String> extras = inAppMessage.getExtras();
      StringBuilder keyValuePairs = new StringBuilder("Dismissed in-app message with extras payload containing [");
      for (String key : extras.keySet()) {
        keyValuePairs.append(" '").append(key).append(" = ").append(extras.get(key)).append('\'');
      }
      keyValuePairs.append(']');
      Toast.makeText(mActivity, keyValuePairs.toString(), Toast.LENGTH_LONG).show();
    } else {
      Toast.makeText(mActivity, "The in-app message was dismissed.", Toast.LENGTH_LONG).show();
    }
  }

  @Override
  public void beforeInAppMessageViewOpened(View inAppMessageView, IInAppMessage inAppMessage) { }

  @Override
  public void afterInAppMessageViewOpened(View inAppMessageView, IInAppMessage inAppMessage) { }

  @Override
  public void beforeInAppMessageViewClosed(View inAppMessageView, IInAppMessage inAppMessage) { }

  @Override
  public void afterInAppMessageViewClosed(IInAppMessage inAppMessage) { }
}
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
class CustomInAppMessageManagerListener(private val mActivity: Activity) : IInAppMessageManagerListener {
  override fun onInAppMessageReceived(inAppMessage: IInAppMessage): Boolean {
    return false
  }
  override fun beforeInAppMessageDisplayed(inAppMessage: IInAppMessage): InAppMessageOperation {
    return InAppMessageOperation.DISPLAY_NOW
  }
  override fun onInAppMessageClicked(inAppMessage: IInAppMessage, inAppMessageCloser: InAppMessageCloser): Boolean {
    Toast.makeText(mActivity, "The click was ignored.", Toast.LENGTH_LONG).show()
    // Closing should not be animated if transitioning to a new activity.
    // If remaining in the same activity, closing should be animated.
    inAppMessageCloser.close(true)
    return true
  }
  override fun onInAppMessageButtonClicked(inAppMessage: IInAppMessage, button: MessageButton, inAppMessageCloser: InAppMessageCloser): Boolean {
    Toast.makeText(mActivity, "The button click was ignored.", Toast.LENGTH_LONG).show()
    // Closing should not be animated if transitioning to a new activity.
    // If remaining in the same activity, closing should be animated.
    inAppMessageCloser.close(true)
    return true
  }
  override fun onInAppMessageDismissed(inAppMessage: IInAppMessage) {
    if (inAppMessage.extras != null && !inAppMessage.extras!!.isEmpty()) {
      val extras = inAppMessage.extras
      val keyValuePairs = StringBuilder("Dismissed in-app message with extras payload containing [")
      for (key in extras!!.keys) {
        keyValuePairs.append(" '").append(key).append(" = ").append(extras[key]).append('\'')
      }
      keyValuePairs.append(']')
      Toast.makeText(mActivity, keyValuePairs.toString(), Toast.LENGTH_LONG).show()
    } else {
      Toast.makeText(mActivity, "The in-app message was dismissed.", Toast.LENGTH_LONG).show()
    }
  }
  override fun beforeInAppMessageViewOpened(inAppMessageView: View, inAppMessage: IInAppMessage) {}
  override fun afterInAppMessageViewOpened(inAppMessageView: View, inAppMessage: IInAppMessage) {}
  override fun beforeInAppMessageViewClosed(inAppMessageView: View, inAppMessage: IInAppMessage) {}
  override fun afterInAppMessageViewClosed(inAppMessage: IInAppMessage) {}
}
```

{% endtab %}
{% endtabs %}

#### Step 2: Instruct Braze to use your In-App Message Manager Listener

Once your `IInAppMessageManagerListener` is created, call `AppboyInAppMessageManager.getInstance().setCustomInAppMessageManagerListener()` to instruct `AppboyInAppMessageManager`
to use your custom `IInAppMessageManagerListener` instead of the default listener.

>  We recommend setting your `IInAppMessageManagerListener` in your [`Application.onCreate()`][82] before any other calls to Braze. This will ensure that the custom listener is set before any in-app message is displayed.

#### In-Depth: Altering In-App Messages Before Display

When a new in-app message is received, and there is already an in-app message being displayed, the new message will be put onto the top of the stack and can be displayed at a later time.

However, if there is no in-app message being displayed, the following delegate method in `IInAppMessageManagerListener` will be called:

```java
  @Override
  public InAppMessageOperation beforeInAppMessageDisplayed(IInAppMessage inAppMessageBase) {
    return InAppMessageOperation.DISPLAY_NOW;
  }
```

The `InAppMessageOperation()` return value can be used to control when the message should be displayed. The suggested usage of this method would be to delay messages in certain parts of the app by returning `DISPLAY_LATER` when in-app messages would be distracting to the user's app experience.

| `InAppMessageOperation` return value | Behavior |
| -------------------------- | -------- |
| `DISPLAY_NOW` | The message will be displayed |
| `DISPLAY_LATER` | The message will be returned to the stack and displayed at the next available opportunity |
| `DISCARD` | The message will be discarded |
| `null` | The message will be ignored. This method should __NOT__ return `null` |
{: .reset-td-br-1 .reset-td-br-2}

See [`InAppMessageOperation.java`][45] for more details.

>  If you choose to `DISCARD` the in-app message and replace it with your own in-app message view, you will need to manually log in-app message clicks and impressions.

On Android, this is done by calling `logClick` and `logImpression` on in-app messages, and `logButtonClick` on immersive in-app messages.

>  Once an in-app message has been placed on the stack, you can request for it to be retrieved and displayed at any time by calling [`AppboyInAppMessageManager.getInstance().requestDisplayInAppMessage()`][67]. Calling this method requests Braze to display the next available in-app message from the stack.

### Setting a Custom View Factory {#custom-view}

Braze's suite of in-app messages types are versatile enough to cover the vast majority of custom use cases.  However, if you would like to fully define the visual appearance of your in-app messages instead of using a default type, Braze makes this possible via setting a custom view factory.

#### Step 1: Implement an In-App Message View Factory

Create a class that implements [`IInAppMessageViewFactory`][42]

{% tabs %}
{% tab JAVA %}

```java
public class CustomInAppMessageViewFactory implements IInAppMessageViewFactory {
  @SuppressLint("InflateParams")
  @Override
  public View createInAppMessageView(Activity activity, IInAppMessage inAppMessage) {
    CustomInAppMessageView inAppMessageView = (CustomInAppMessageView) activity.getLayoutInflater().inflate(R.layout.custom_inappmessage, null);

    inAppMessageView.setMessageBackgroundColor(inAppMessage.getBackgroundColor());
    inAppMessageView.setMessage(inAppMessage.getMessage());
    inAppMessageView.setMessageTextColor(inAppMessage.getMessageTextColor());
    inAppMessageView.setMessageIcon(inAppMessage.getIcon(), inAppMessage.getIconBackgroundColor(), inAppMessage.getIconColor());
    if (inAppMessage instanceof IInAppMessageWithImage) {
      inAppMessageView.setMessageImage(((IInAppMessageWithImage) inAppMessage).getBitmap());
    }
    inAppMessageView.resetMessageMargins();

    return inAppMessageView;
  }
}
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
class CustomInAppMessageViewFactory : IInAppMessageViewFactory {
  @SuppressLint("InflateParams")
  override fun createInAppMessageView(activity: Activity, inAppMessage: IInAppMessage): View {
    val inAppMessageView = activity.layoutInflater.inflate(R.layout.custom_inappmessage, null) as CustomInAppMessageView
    inAppMessageView.setMessageBackgroundColor(inAppMessage.backgroundColor)
    inAppMessageView.setMessage(inAppMessage.message)
    inAppMessageView.setMessageTextColor(inAppMessage.messageTextColor)
    inAppMessageView.setMessageIcon(inAppMessage.icon, inAppMessage.iconBackgroundColor, inAppMessage.iconColor)
    if (inAppMessage is IInAppMessageWithImage) {
      inAppMessageView.setMessageImage((inAppMessage as IInAppMessageWithImage).bitmap)
    }
    inAppMessageView.resetMessageMargins()
    return inAppMessageView
  }
}
```

{% endtab %}
{% endtabs %}

#### Step 2: Instruct Braze to use your In-App Message View Factory

Once your `IInAppMessageViewFactory` is created, call `AppboyInAppMessageManager.getInstance().setCustomInAppMessageViewFactory()` to instruct `AppboyInAppMessageManager`
to use your custom `IInAppMessageViewFactory` instead of the default view factory.

>  We recommend setting your `IInAppMessageViewFactory` in your [`Application.onCreate()`][82] before any other calls to Braze. This will ensure that the custom view factory is set before any in-app message is displayed.

#### In-Depth: Implementing a Braze View Interface

Braze's `slideup` in-app message view implements [`IInAppMessageView`][25].  Braze's `full` and `modal` type message views implement [`IInAppMessageImmersiveView`][24].  Implementing one of these classes will allow Braze to add click listeners to your custom view where appropriate.  All Braze view classes extend Android's [View][18] class.

Implementing `IInAppMessageView` allows you to define a certain portion of your custom view as clickable.  Implementing `IInAppMessageImmersiveView` allows you to define message button views and a close button view.

### Setting a Custom Animation Factory

In-app messages have preset animation behavior. `Slideup` type messages slide into the screen; `full` and `modal` messages fade in and out.  If you would like to define custom animation behaviors for your in-app messages, Braze makes this possible via setting a custom animation factory.

#### Step 1: Implement an In-App Message Animation Factory

Create a class that implements [`IInAppMessageAnimationFactory`][20]. Example implementation below:

{% tabs %}
{% tab JAVA %}

```java
public class CustomInAppMessageAnimationFactory implements IInAppMessageAnimationFactory {

  @Override
  public Animation getOpeningAnimation(IInAppMessage inAppMessage) {
    Animation animation = new AlphaAnimation(0, 1);
    animation.setInterpolator(new AccelerateInterpolator());
    animation.setDuration(2000L);
    return animation;
  }

  @Override
  public Animation getClosingAnimation(IInAppMessage inAppMessage) {
    Animation animation = new AlphaAnimation(1, 0);
    animation.setInterpolator(new DecelerateInterpolator());
    animation.setDuration(2000L);
    return animation;
  }
}
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
class CustomInAppMessageAnimationFactory : IInAppMessageAnimationFactory {
  override fun getOpeningAnimation(inAppMessage: IInAppMessage): Animation {
    val animation: Animation = AlphaAnimation(0, 1)
    animation.interpolator = AccelerateInterpolator()
    animation.duration = 2000L
    return animation
  }

  override fun getClosingAnimation(inAppMessage: IInAppMessage): Animation {
    val animation: Animation = AlphaAnimation(1, 0)
    animation.interpolator = DecelerateInterpolator()
    animation.duration = 2000L
    return animation
  }
}
```

{% endtab %}
{% endtabs %}

#### Step 2: Instruct Braze to use your In-App Message View Factory

Once your `IInAppMessageAnimationFactory` is created, call `AppboyInAppMessageManager.getInstance().setCustomInAppMessageAnimationFactory()` to instruct `AppboyInAppMessageManager`
to use your custom `IInAppMessageAnimationFactory` instead of the default animation factory.

>  We recommend setting your `IInAppMessageAnimationFactory` in your [`Application.onCreate()`][82] before any other calls to Braze. This will ensure that the custom animation factory is set before any in-app message is displayed.

## Setting Fixed Orientation

To set a fixed orientation for an in-app message, first [set a custom in-app message manager listener][19]. Then, call `setOrientation()` on the `IInAppMessage` object in the `beforeInAppMessageDisplayed()` delegate method.

```java
public InAppMessageOperation beforeInAppMessageDisplayed(IInAppMessage inAppMessage) {
  // Set the orientation to portrait
 inAppMessage.setOrientation(Orientation.PORTRAIT);
 return InAppMessageOperation.DISPLAY_NOW;
}
```

## GIFs {#gifs-IAMs}

{% include archive/android/gifs.md channel="in-app messages" %}

## Advanced Notes
### Android Dialogs

Braze doesn't support displaying in-app messages in [Android Dialogs][39] at this time.

### Button Text Capitalization

Android Material Design specifies that Button text should be upper case by default. Braze's in-app message buttons follow this convention as well.

### Youtube in HTML in-app messages

Starting in Braze Android SDK version 2.0.1, Youtube and other HTML5 content can play in HTML in-app messages. This requires hardware acceleration to be enabled in the Activity where the in-app message is being displayed, please see the [Android developer guide][71] for more details. Also, that hardware acceleration is only available on API versions 11 and above.

[5]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/in-app_messaging/
[6]: https://github.com/Appboy/appboy-android-sdk/blob/master/android-sdk-ui/res/values/styles.xml
[12]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/in-app_messaging/#setting-a-custom-view-factory
[14]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/news_feed/#key-value-pairs
[15]: http://fortawesome.github.io/Font-Awesome/
[18]: http://developer.android.com/reference/android/view/View.html
[19]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/in-app_messaging/#setting-a-custom-manager-listener
[20]: https://github.com/Appboy/appboy-android-sdk/blob/master/android-sdk-ui/src/com/appboy/ui/inappmessage/IInAppMessageAnimationFactory.java
[21]: https://github.com/Appboy/appboy-android-sdk/blob/master/android-sdk-ui/src/com/appboy/ui/inappmessage/listeners/IInAppMessageManagerListener.java
[22]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/in-app_messaging/#setting-a-custom-animation-factory
[24]: https://github.com/Appboy/appboy-android-sdk/blob/master/android-sdk-ui/src/com/appboy/ui/inappmessage/IInAppMessageImmersiveView.java
[25]: https://github.com/Appboy/appboy-android-sdk/blob/master/android-sdk-ui/src/com/appboy/ui/inappmessage/IInAppMessageView.java
[34]: https://github.com/Appboy/appboy-android-sdk/blob/master/android-sdk-ui/src/com/appboy/ui/inappmessage/AppboyInAppMessageManager.java
[39]: https://developer.android.com/guide/topics/ui/dialogs.html
[42]: https://github.com/Appboy/appboy-android-sdk/blob/master/android-sdk-ui/src/com/appboy/ui/inappmessage/IInAppMessageViewFactory.java
[44]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/models/IInAppMessage.html#getExtras()
[45]: https://github.com/Appboy/appboy-android-sdk/blob/master/android-sdk-ui/src/com/appboy/ui/inappmessage/InAppMessageOperation.java
[67]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/ui/inappmessage/AppboyInAppMessageManager.html#requestDisplayInAppMessage--
[71]: https://developer.android.com/guide/topics/graphics/hardware-accel.html#controlling
[79]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/advanced_use_cases/font_customization/#font-customization
[82]: https://developer.android.com/reference/android/app/Application.html#onCreate()
[83]: https://github.com/Appboy/appboy-android-sdk/blob/master/android-sdk-ui/src/com/appboy/ui/inappmessage/InAppMessageOperation.java
