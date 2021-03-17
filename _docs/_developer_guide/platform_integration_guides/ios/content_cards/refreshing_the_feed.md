---
nav_title: Refreshing the Feed
platform: iOS
page_order: 4

---

# Refreshing the Feed

## Refreshing Content Cards

You can manually request Braze to refresh the user's Content Cards using the `requestContentCardsRefresh:` method on the `Appboy` interface. For example:

{% tabs %}
{% tab OBJECTIVE-C %}

```objc
[[Appboy sharedInstance] requestContentCardsRefresh];
```

{% endtab %}
{% tab swift %}

```swift
Appboy.sharedInstance()?.requestContentCardsRefresh()
```

{% endtab %}
{% endtabs %}

For more information see the [`Appboy.h` header file](https://github.com/Appboy/appboy-ios-sdk/blob/master/AppboyKit/headers/AppboyKitLibrary/Appboy.h).
