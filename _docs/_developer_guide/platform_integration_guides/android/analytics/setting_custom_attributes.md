---
nav_title: Setting Custom Attributes
platform: Android
page_order: 3

---
## Setting Custom Attributes

Braze provides methods for assigning attributes to users. You'll be able to filter and segment your users according to these attributes on the dashboard.

Before implementation, be sure to review examples of the segmentation options afforded by Custom events vs. Custom attributes vs Purchase events in our [Analytics Overview][7], as well as our notes on [event naming conventions]({{site.baseurl}}/user_guide/data_and_analytics/custom_data/event_naming_conventions/).

### Assigning Standard User Attributes

To assign attributes to your users, call the `getCurrentUser()` method on your Braze instance to get a reference to the current user of your app. Once you have a reference to the current user, you can call methods to set predefined or custom attributes.

Braze provides predefined methods for setting the following user attributes within the [AppboyUser class][2]. See the [Javadocs for method specifications][2]:

- First Name
- Last Name
- Country
- Language
- Date of Birth
- Email
- Avatar Image URLs for Braze User Profiles
- Gender
- Home City
- Phone Number
- Facebook Data
- Twitter Data

All string values such as first name, last name, country, and home city are limited to 255 characters. Avatar Image URLs are limited to 1024 characters.

**Implementation Example**
This is what setting a first name would look like in code:

{% tabs %}
{% tab JAVA %}

```java
Appboy.getInstance(context).getCurrentUser().setFirstName("first_name");
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
Appboy.getInstance(context).currentUser?.setFirstName("first_name")
```

{% endtab %}
{% endtabs %}

### Assigning Custom User Attributes

In addition to our predefined user attribute methods, Braze also provides custom attributes to track data from your applications. Braze Custom Attributes can be set with the following data types:

- Strings
- Arrays
  - Includes methods to set arrays, add items to existing arrays, and delete items from existing arrays.
- Integers
- Booleans
- Dates
- Longs
- Floats
- Doubles

Full method specifications for custom attributes can be found here within the [AppboyUser class within the Javadocs][2].

#### Setting a Custom Attribute with a String Value

{% tabs %}
{% tab JAVA %}

```java
Appboy.getInstance(context).getCurrentUser().setCustomUserAttribute("your_attribute_key", "your_attribute_value");
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
Appboy.getInstance(context).currentUser?.setCustomUserAttribute("your_attribute_key", "your_attribute_value")
```

{% endtab %}
{% endtabs %}

#### Setting a Custom Attribute with an Integer Value

{% tabs %}
{% tab JAVA %}

```java
Appboy.getInstance(context).getCurrentUser().setCustomUserAttribute, "your_attribute_key", YOUR_INT_VALUE);
// Integer attributes may also be incremented using code like the following:
Appboy.getInstance(context).getCurrentUser().incrementCustomUserAttribute("your_attribute_key", YOUR_INCREMENT_VALUE);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
Appboy.getInstance(context).currentUser?.setCustomUserAttribute, "your_attribute_key", YOUR_INT_VALUE)
// Integer attributes may also be incremented using code like the following:
Appboy.getInstance(context).currentUser?.incrementCustomUserAttribute("your_attribute_key", YOUR_INCREMENT_VALUE)
```

{% endtab %}
{% endtabs %}

#### Setting a Custom Attribute with a Boolean Value

{% tabs %}
{% tab JAVA %}

```java
Appboy.getInstance(context).getCurrentUser().setCustomUserAttribute("your_attribute_key", YOUR_BOOLEAN_VALUE);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
Appboy.getInstance(context).currentUser?.setCustomUserAttribute("your_attribute_key", YOUR_BOOLEAN_VALUE)
```

{% endtab %}
{% endtabs %}

#### Setting a Custom Attribute with a Long Value

{% tabs %}
{% tab JAVA %}

```java
Appboy.getInstance(context).getCurrentUser().setCustomUserAttribute("your_attribute_key", YOUR_LONG_VALUE);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
Appboy.getInstance(context).currentUser?.setCustomUserAttribute("your_attribute_key", YOUR_LONG_VALUE)
```

{% endtab %}
{% endtabs %}

#### Setting a Custom Attribute with a Float Value

{% tabs %}
{% tab JAVA %}

```java
Appboy.getInstance(context).getCurrentUser().setCustomUserAttribute("your_attribute_key", YOUR_FLOAT_VALUE);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
Appboy.getInstance(context).currentUser?.setCustomUserAttribute("your_attribute_key", YOUR_FLOAT_VALUE)
```

{% endtab %}
{% endtabs %}

#### Setting a Custom Attribute with a Double Value

{% tabs %}
{% tab JAVA %}

```java
Appboy.getInstance(context).getCurrentUser().setCustomUserAttribute("your_attribute_key", YOUR_DOUBLE_VALUE);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
Appboy.getInstance(context).currentUser?.setCustomUserAttribute("your_attribute_key", YOUR_DOUBLE_VALUE)
```

{% endtab %}
{% endtabs %}

#### Setting a Custom Attribute with a Date Value

{% tabs %}
{% tab JAVA %}

```java
Appboy.getInstance(context).getCurrentUser().setCustomUserAttribute("your_attribute_key", YOUR_DATE_VALUE);
// This method will assign the current time to a custom attribute at the time the method is called:
Appboy.getInstance(context).getCurrentUser().setCustomUserAttributeToNow("your_attribute_key");
// This method will assign the date specified by SECONDS_FROM_EPOCH to a custom attribute:
Appboy.getInstance(context).getCurrentUser().setCustomUserAttributeToSecondsFromEpoch("your_attribute_key", SECONDS_FROM_EPOCH);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
Appboy.getInstance(context).currentUser?.setCustomUserAttribute("your_attribute_key", YOUR_DATE_VALUE)
// This method will assign the current time to a custom attribute at the time the method is called:
Appboy.getInstance(context).currentUser?.setCustomUserAttributeToNow("your_attribute_key")
// This method will assign the date specified by SECONDS_FROM_EPOCH to a custom attribute:
Appboy.getInstance(context).currentUser?.setCustomUserAttributeToSecondsFromEpoch("your_attribute_key", SECONDS_FROM_EPOCH)
```

{% endtab %}
{% endtabs %}

{% alert warning %}
  Dates passed to Braze with this method must either be in the [ISO 8601](http://en.wikipedia.org/wiki/ISO_8601) format (e.g `2013-07-16T19:20:30+01:00`) or in the `yyyy-MM-dd'T'HH:mm:ss:SSSZ` format (e.g `2016-12-14T13:32:31.601-0800`).
{% endalert %}

#### Setting a Custom Attribute with an Array Value
The maximum number of elements in Custom Attribute Arrays defaults to 25. The maximum for individual arrays can be increased to up to 100 in the Braze Dashboard, under "Manage App Group -> Custom Attributes". Arrays exceeding the maximum number of elements will be truncated to contain the maximum number of elements. For more information on Custom Attribute Arrays and their behavior, see our [Documentation on Arrays][6].

{% tabs %}
{% tab JAVA %}

```java
// Setting a custom attribute with an array value
Appboy.getInstance(context).getCurrentUser().setCustomAttributeArray("your_attribute_key", testSetArray);
// Adding to a custom attribute with an array value
Appboy.getInstance(context).getCurrentUser().addToCustomAttributeArray("your_attribute_key", "value_to_add");
// Removing a value from an array type custom attribute
Appboy.getInstance(context).getCurrentUser().removeFromCustomAttributeArray("your_attribute_key", "value_to_remove");
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
// Setting a custom attribute with an array value
Appboy.getInstance(context).currentUser?.setCustomAttributeArray("your_attribute_key", testSetArray)
// Adding to a custom attribute with an array value
Appboy.getInstance(context).currentUser?.addToCustomAttributeArray("your_attribute_key", "value_to_add")
// Removing a value from an array type custom attribute
Appboy.getInstance(context).currentUser?.removeFromCustomAttributeArray("your_attribute_key", "value_to_remove")
```

{% endtab %}
{% endtabs %}

#### Unsetting a Custom Attribute

Custom Attributes can also be unset using the following method:

{% tabs %}
{% tab JAVA %}

```java
Appboy.getInstance(context).getCurrentUser().unsetCustomUserAttribute("your_attribute_key");
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
Appboy.getInstance(context).currentUser?.unsetCustomUserAttribute("your_attribute_key")
```

{% endtab %}
{% endtabs %}

#### Setting a Custom Attribute via the REST API

You can also use our REST API to set user attributes. To do so refer to the [User API documentation][4].

#### Custom Attribute Length

Custom attribute keys and values have a maximum length of 255 characters.  Longer strings will be truncated to 255 characters.

Full class information can be found in the [javadocs][2].

### Setting Up User Subscriptions

To set up a subscription for your users (either email or push), call the functions `setEmailNotificationSubscriptionType()`  or `setPushNotificationSubscriptionType()`, respectively. Both of these functions take the enum type 'NotificationSubscriptionType' as arguments. This type has three different states:

| Subscription Status | Definition |
| ------------------- | ---------- |
| `OPTED_IN` | Subscribed, and explicitly opted in |
| `SUBSCRIBED` | Subscribed, but not explicitly opted in |
| `UNSUBSCRIBED` | Unsubscribed and/or explicitly opted out |
{: .reset-td-br-1 .reset-td-br-2}

{% alert important %}
No explicit opt-in is required by Android to send users push notifications. When a user is registered for push, they are set to `SUBSCRIBED` rather than `OPTED_IN` by default. For more information on implementing subscriptions and explicit opt-ins, visit the topic in our [User Guide]({{site.baseurl}}/user_guide/message_building_by_channel/email/managing_user_subscriptions/#managing-user-subscriptions).
{% endalert %}

#### Sample Code

##### Setting Email Subscriptions

{% tabs %}
{% tab JAVA %}

```java
Appboy.getInstance(context).getCurrentUser().setEmailNotificationSubscriptionType(emailNotificationSubscriptionType);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
Appboy.getInstance(context).currentUser?.setEmailNotificationSubscriptionType(emailNotificationSubscriptionType)
```

{% endtab %}
{% endtabs %}

##### Setting Push Notification Subscription

{% tabs %}
{% tab JAVA %}

```java
Appboy.getInstance(context).getCurrentUser().setPushNotificationSubscriptionType(pushNotificationSubscriptionType);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
Appboy.getInstance(context).currentUser?.setPushNotificationSubscriptionType(pushNotificationSubscriptionType)
```

{% endtab %}
{% endtabs %}

[2]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/AppboyUser.html "Javadocs"
[4]: {{site.baseurl}}/developer_guide/rest_api/user_data/#user-data
[6]: {{site.baseurl}}/developer_guide/platform_wide/analytics_overview/#arrays
[7]: {{site.baseurl}}/developer_guide/platform_wide/analytics_overview/#user-data-collection
