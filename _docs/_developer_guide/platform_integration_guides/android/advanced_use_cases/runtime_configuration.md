---
nav_title: Runtime Configuration
platform: Android
page_order: 4

---
## Runtime Configuration

Runtime configuration is an optional way to configure your app at runtime in place of an `braze.xml`. The use of both runtime configuration and `braze.xml` configuration is still possible. Runtime configured values will always take precedence over the same value in the `braze.xml`. If the Braze SDK can find all values in the runtime configuration, then the `braze.xml` is no longer needed and can be removed. See the full documentation [here][1].

### Example Usage

The configuration uses a [builder object][2] that is then built and passed to [Appboy.configure()][1]. The following example uses a subset of the runtime configuration options available, see the [javadoc][1] for a complete list of available configuration options.

{% tabs %}
{% tab JAVA %}

```java
AppboyConfig appboyConfig = new AppboyConfig.Builder()
        .setApiKey("api-key-here")
        .setCustomEndpoint("YOUR_CUSTOM_ENDPOINT_OR_CLUSTER")
        .setSessionTimeout(60)
        .setHandlePushDeepLinksAutomatically(true)
        .setGreatNetworkDataFlushInterval(10)
        .build();
Appboy.configure(this, appboyConfig);
```

{% endtab %}
{% tab KOTLIN %}

```kotlin
val appboyConfig = AppboyConfig.Builder()
        .setApiKey("api-key-here")
        .setCustomEndpoint("YOUR_CUSTOM_ENDPOINT_OR_CLUSTER")
        .setSessionTimeout(60)
        .setHandlePushDeepLinksAutomatically(true)
        .setGreatNetworkDataFlushInterval(10)
        .build()
Appboy.configure(this, appboyConfig)
```

{% endtab %}
{% endtabs %}

Another example can be found in our [Hello Appboy sample app][3].

[1]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/Appboy.html#configure-android.content.Context-com.appboy.configuration.AppboyConfig-
[2]: https://appboy.github.io/appboy-android-sdk/javadocs/com/appboy/configuration/AppboyConfig.Builder.html
[3]: https://github.com/Appboy/appboy-android-sdk/blob/master/hello-appboy/src/main/java/com/appboy/helloworld/HelloAppboyApplication.java#L25
