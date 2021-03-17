---
nav_title: Inline Image Push
platform: Android
page_order: 14

---

# Android Inline Image Push

![Android Inline Image Push example]({% image_buster /assets/img/android/push/inline_image_push_android_1.png %}){: style="float:right;max-width:30%;margin-left:15px;border: 0;"}

> Showcase a larger image within your Android push notification using Inline Image push. With this design, users won't have to manually expand the push to enlarge the image.

# Usage Requirements

{% alert tip %}
No additional integration or SDK changes are required to use this feature. Devices or SDKs which don't meet the minimum version requirements will instead show a standard big image push notification.
{% endalert %}

- This notification type requires the Braze Android SDK v10.0.0+ and Android M+ devices. 
- Unsupported devices or SDKs will fallback to the standard big image push notification.
- Unlike regular Android push notifications, Inline Image push images are in a 3:2 aspect ratio.


This feature is available in the **Notification Type** menu when creating an Android Push message.

![Android Inline Image Push example]({% image_buster /assets/img/android/push/android_inline_image_notification_type.png %})
