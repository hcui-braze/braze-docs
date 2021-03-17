---
nav_title: Tracking Custom Events
platform: iOS
page_order: 2

---
# Tracking Custom Events

You can record custom events in Braze to learn more about your app's usage patterns and to segment your users by their actions on the dashboard.

Before implementation, be sure to review examples of the segmentation options afforded by Custom events vs. Custom attributes vs Purchase events in our [Best Practices section][0], as well as our notes on [event naming conventions]({{site.baseurl}}/user_guide/data_and_analytics/custom_data/event_naming_conventions/).

## Adding A Custom Event

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance] logCustomEvent:@"YOUR_EVENT_NAME"];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.logCustomEvent("YOUR_EVENT_NAME")
```

{% endtab %}
{% endtabs %}

### Adding Properties

You can add metadata about custom events by passing an `NSDictionary` populated with `NSNumber`, `NSString`, or `NSDate` values.

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance] logCustomEvent:@"YOUR_EVENT_NAME" withProperties:@{@"key1":"value1"}];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.logCustomEvent("YOUR_EVENT_NAME", withProperties:["key1":"value1"])
```

{% endtab %}
{% endtabs %}

See our [class documentation][4] for more information.

### Reserved Keys {#event-reserved-keys}

The following keys are __RESERVED__ and __CANNOT__ be used as Custom event properties:

- `time`
- `event_name`

**Additional Information**

- See the method declaration within the [`Appboy.h` file][2]. - In addition, you may refer to the [logCustomEvent Documentation][3] for more information.

[0]: {{site.baseurl}}/developer_guide/platform_wide/analytics_overview/#user-data-collection
[2]: https://github.com/Appboy/appboy-ios-sdk/blob/master/AppboyKit/headers/AppboyKitLibrary/Appboy.h
[3]: http://appboy.github.io/appboy-ios-sdk/docs/interface_appboy.html#ad80c39e8c96482a77562a5b1a1d387aa "logcustomevent documentation"
[4]: http://appboy.github.io/appboy-ios-sdk/docs/interface_appboy.html#a4f0051d73d85cb37f63c232248124c79 "logcustomevent:withproperties documentation"
