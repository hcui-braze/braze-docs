---
nav_title: SDK Integration
platform: Unity
subplatform: iOS
page_order: 0
---
# SDK Integration

Installing the Braze SDK will provide you with the ability to collect analytics and engage users with push notifications and native in-app messages.

Before you can start using Braze in your Unity scripts, you'll need to import the plugin files to your Unity project.

## Initial SDK Setup

Follow the below instructions to get Braze running in your Unity application. If you are transitioning from a manual integration, please read the instructions on [Transitioning From a Manual to an Automated Integration][5].

## Step 1: Choose your Braze Unity Package

The Braze [`.unitypackage`][41] bundles native bindings for the Android and iOS platforms, along with a Unity C# interface.

There are several Braze Unity packages available for download at [Braze Unity Releases Page][42]:
 
{% include archive/unity/unitypackage_descriptions.md%}

## Step 2: Import the Package

1. In the Unity Editor, import the package into your Unity project by navigating to `Assets > Import Package > Custom Package`.
2. Click "Import".

Alternatively, follow the Unity instructions for [Importing Asset packages][41] for a more detailed guide on importing custom Unity packages. 

Braze also provides the option of [customizing and exporting the Unity package][37].

{% alert note %}
If you only wish to import the iOS plugin, deselect the `Plugins/Android` subdirectory when importing the Braze `.unitypackage`.
{% endalert %}

### Step 3: Set Your API Key

Braze provides a native Unity solution for automating the Unity iOS integration. This solution modifies the built Xcode project using Unity's [`PostProcessBuildAttribute`][6] and subclasses the UnityAppController using the `IMPL_APP_CONTROLLER_SUBCLASS` macro.

1. In the Unity Editor, open the Braze Configuration Settings by navigating to Braze > Braze Configuration.
2. Check the "Automate Unity iOS Integration" box.
3. In the "Braze API Key" field, input your application's API key from the [Braze Dashboard][19]. Your Braze Configuration settings should look like this:

![Braze Config Editor][18]

>  If your application is already using another `UnityAppController` subclass, you will need to merge your subclass implementation with `AppboyAppDelegate.mm`.

### Step 4: SDK Integration Complete

Braze should now be collecting data from your application and your basic integration should be complete. Continue on to the following sections to integrate the complete suite of Braze features.

## In-App Message Integration

By default, Braze will handle and display in-app messages via the native iOS SDK. If you wish to pass in-app messages to Unity or customize in-app message display and handling, you must set an in-app message listener by doing the following:

1. Please ensure that you have followed the Initial SDK Setup steps on [setting your Braze API key][8] through Unity.
	- If you would like to do this integration manually, please refer to the instructions on [Manual In-App Message Integration][26].
2. Set the name of your Game Object and In-App Message listener callback method under "Set In-App Message Listener."
3. If you set a listener, Braze will send the in-app message to Unity instead of displaying it, and your in-app message will __not__ automatically appear in your app. If you check "Braze Displays In-App Messages", Braze will both send the in-app message to your callback method and display it.

![In-App Message Listener][25]

### Implementation Example {#iam-implementation-example}

For a sample implementation, take a look at the [`InAppMessageReceivedCallback`][27] in `AppboyBindingTester.cs`.

### In-App Message Integration Complete

Your Unity application is now set up to receive in-app messages from Braze. See the [In-App Message documentation][34] for information on in-app message customization.

## Content Cards Integration

Braze's Content Cards feature allows you to insert content directly into your app from our web dashboard. Braze does not provide a default UI for Content Cards in Unity. Instead, the Braze SDK passes along all cards to Unity. If you wish to integrate Content Cards into your application, you must do the following:

1. Ensure that you have followed the Initial SDK Setup steps on [setting your Braze API key][8] through Unity.
	- If you would like to integrate Content Cards manually, please refer to the instructions on [Manual Content Cards Integration][29].
2. Set the name of your Game Object and Content Cards listener callback method under "Set Content Cards Listener."

![Set Content Cards Listener][40]

### Implementation Example {#feed-implementation-example}

The method `ContentCardsReceivedCallback` in our [sample callback code][38] shows an example of parsing incoming Content Card data into our convenience wrapper class for Content Cards, [`ContentCard.cs`][39]. `ContentCard.cs` also supports logging analytics through its `LogImpression()` and `LogClick()` methods.

Sample code for parsing incoming Content Card data:

```
void ExampleCallback(string message) {
	// Example of logging a Content Card displayed event
	AppboyBinding.LogContentCardsDisplayed();
	try {
		JSONClass json = (JSONClass)JSON.Parse(message);

		// Content Card data is contained in the `mContentCards` field of the top level object.
		if (json["mContentCards"] != null) {
			JSONArray jsonArray = (JSONArray)JSON.Parse(json["mContentCards"].ToString());
			Debug.Log(String.Format("Parsed content cards array with {0} cards", jsonArray.Count));

			// Iterate over the card array to parse individual cards.
			for (int i = 0; i < jsonArray.Count; i++) {
				JSONClass cardJson = jsonArray[i].AsObject;
				try {
					ContentCard card = new ContentCard(cardJson);
					Debug.Log(String.Format("Created card object for card: {0}", card));

					// Example of logging Content Card analytics on the ContentCard object 
					card.LogImpression();
					card.LogClick();
				} catch {
					Debug.Log(String.Format("Unable to create and log analytics for card {0}", cardJson));
				}
			}
		}
	} catch {
		throw new ArgumentException("Could not parse content card JSON message.");
	}
}
```

### Content Cards Integration Complete

## News Feed Integration

Braze's News Feed allows you to insert permanent content directly into your app from our web dashboard. Braze does not provide a default UI for the News Feed in Unity. Instead, the Braze SDK passes along all News Feed cards to Unity. If you wish to integrate the News Feed into your application, you must do the following:

1. Ensure that you have followed the Initial SDK Setup steps on [setting your Braze API key][8] through Unity.
	- If you would like to integrate the News Feed manually, please refer to the instructions on [Manual News Feed Integration][28].
2. Set the name of your Game Object and News Feed listener callback method under "Set News Feed Listener."

![Set News Feed Listener][30]

### Implementation Example {#feed-implementation-example}

For a sample implementation, take a look at the [`FeedReceivedCallback`][36] in `AppboyBindingTester.cs`.

### News Feed Integration Complete

Your Unity application is now set up to receive the News Feed data model from Braze. Next, see the [News Feed documentation][35] for information on requesting a News Feed data refresh and logging News Feed analytics.

## Transitioning from Manual to Automated Integration

To take advantage of the automated iOS integration offered in the Braze Unity SDK, follow these steps on transitioning from a manual to an automated integration.

1. Remove all Braze-related code from your Xcode project's `UnityAppController` subclass.
2. Remove Braze's iOS libraries from your Unity or Xcode project (i.e., `Appboy_iOS_SDK.framework` and `SDWebImage.framework`) and [import the Braze Unity package][7] into your Unity project.
3. Follow the integration instructions on [setting your API key through Unity][8].

## Manual SDK Integration

### Initial SDK Setup {#manual-initial-sdk-setup}

Follow the instructions below to manually integrate Braze into your built Xcode project.

#### Step 1: Clone the Unity SDK

Clone the [Braze Unity SDK Github project][1]

```bash
git clone git@github.com:Appboy/appboy-unity-sdk.git
```

#### Step 2: Copy Required Plugins

1. `Plugins/Appboy` -> `/<your-project>/Assets/Plugins/Appboy`
2. `Plugins/iOS` -> `/<your-project>/Assets/Plugins/iOS`

#### Step 3: Test Xcode Project Generatino

Now that you've copied over the requisite plugins, the following steps will help you generate your Xcode project and add the required classes for the Braze SDK to function:

1. Generate your Xcode project in Unity by clicking on File > Build Settings...
2. Select iOS as the platform and click "Build".
3. Select `/your-project/your-iOS-project/` as the build location.
4. Confirm that Unity has copied the source files from `Plugins/iOS` to your generated project under the "Libraries" directory.

#### Step 4: Add the Braze iOS SDK framework and required system frameworks

You must now add the Braze iOS SDK framework into your project.

1. Add `Appboy_iOS_SDK.framework` and `SDWebImage.framework` to your project as embedded frameworks.
  - Your "Build Phases" panel's "Embed Frameworks" step should contain `Appboy_iOS_SDK.framework` and `SDWebImage.framework`
2. Add Required iOS Libraries
	1. Click on the target for your project (using the left-side navigation), and select the “Build Phases” tab
	2. Click the + button under “Link Binary With Libraries”
	3. In the menu, select `SystemConfiguration.framework` and press the Add button.
    ![Add SystemConfiguration.framework][10]
	4. Mark this library as "Required" using the pull-down menu next to `SystemConfiguration.framework`
    ![Mark SystemConfiguration.framework as Required][11]
	5. Repeat Steps 3 and 4 to add each of the following required frameworks to your project, marking each as “Required”
		- `QuartzCore.framework`
		- `CoreImage.framework`
		- `libz.tbd`
    ![Rinse and Repeat][12]
	6. Add the following frameworks and mark them as "Optional":
		- `CoreTelephony.framework`
		- `ImageIO.framework`
		- `Accounts.framework`
		- `StoreKit.framework`
		- `CoreLocation.framework`

#### Step 5: Update Project Build Settings

   1. Click on the target for your project (using the left-side navigation), and select the “Build Settings" tab
   2. Find "Other Linker Flags" and add `-ObjC` to the row

#### Step 5: Initialize the SDK

You now must make modifications to your `UnityAppController` subclass. Typically, this is generated in `Classes/UnityAppController.mm`:

1. At the top of your `UnityAppController` subclass, add the following import statements:

```objc
	#import <Appboy_iOS_SDK/AppboyKit.h>
	#import "AppboyUnityManager.h"
```

2. In the `UIApplicationDelegate` method `applicationDidFinishLaunchingWithOptions`, add the following code snippet above the return statement. Be sure to replace `"YOUR-API-KEY"` with the Braze API key from the [Braze dashboard][19].

```objc
	[Appboy startWithApiKey:@"YOUR-API-KEY"
    	    inApplication:application
        	withLaunchOptions:launchOptions];
```

#### Step 6: SDK Integration Complete

Braze should now be collecting data from your application and your basic integration should be complete. Continue on to the following sections to integrate the complete suite of Braze features.

### In-App Message Integration {#manual-iam-integration}

You can set an in-app message listener by manually modifying your built Xcode project. In order to pass in-app messages from Braze to Unity, you must add the following code to your `applicationDidFinishLaunchingWithOptions` method within your `UnityAppController` subclass:

```objc
[Appboy sharedInstance].inAppMessageController.delegate = [AppboyUnityManager sharedInstance];

[[AppboyUnityManager sharedInstance] addInAppMessageListenerWithObjectName:@"Your Unity Game Object Name" callbackMethodName:@"Your Unity Callback Method Name"];
```

- `@"Your Unity Game Object Name"` must be replaced with the Unity object name you want to listen to the in-app message.
- `@"Your Unity Callback Method Name"` is the callback method name that handles the in-app message.
- The callback method must be contained within the Unity object you passed in as the first parameter.

>  If you have added an in-app message listener, Braze will send the message to Unity instead of displaying it by default, meaning your in-app message will __not__ automatically appear in your app. To have Braze handle displaying in-app messages, change the `(BOOL) onInAppMessageReceived:(ABKInAppMessage *)inAppMessage` method in `AppboyUnityManager.mm` and make it return `NO`.

### News Feed Integration {#manual-feed-integration}

You can set a feed listener by manually modifying your built Xcode project. In order to pass the News Feed from Braze to Unity, you must add the following code to your `applicationDidFinishLaunchingWithOptions` method within your `UnityAppController` subclass:

```objc
[[AppboyUnityManager sharedInstance] addFeedListenerWithObjectName:@"Your Unity Game Object Name" callbackMethodName:@"Your Unity Callback Method Name"];
```

- `@"Your Unity Game Object Name"` must be replaced with the Unity object name you want to receive the News Feed.
- `@"Your Unity Callback Method Name"` is the callback method name that handles the News Feed model.
- The callback method must be contained within the Unity object you passed in as the first parameter.

### Content Cards Integration {#manual-content-cards-integration}

You can set a Content Card listener by manually modifying your built Xcode project. Add the following code to your `applicationDidFinishLaunchingWithOptions` method within your `UnityAppController.mm` file:

```objc
[[AppboyUnityManager sharedInstance] addContentCardsListenerWithObjectName:@"Your Unity Game Object Name" callbackMethodName:@"Your Unity Callback Method Name"];
```

- `@"Your Unity Game Object Name"` must be replaced with the Unity object name you want to receive Content Cards.
- `@"Your Unity Callback Method Name"` is the callback method name that handles the Content Card model.
- The callback method must be contained within the Unity object you passed in as the first parameter.

[1]: https://github.com/appboy/appboy-unity-sdk
[5]: #transitioning-from-manual-to-automated-integration
[6]: http://docs.unity3d.com/ScriptReference/Callbacks.PostProcessBuildAttribute.html
[7]: #step-1-importing-the-braze-unity-package
[8]: #step-2-setting-your-api-key
[9]: {{site.baseurl}}/developer_guide/platform_integration_guides/unity/ios/push_notifications/#step-3-customize-push-notification-settings
[10]: {% image_buster /assets/img_archive/unity-ios-sysconfig.png %}
[11]: {% image_buster /assets/img_archive/unity-ios-sysconfigreq.png %}
[12]: {% image_buster /assets/img_archive/iosunitystep3part5.gif %}
[18]: {% image_buster /assets/img_archive/unity-ios-appboyconfig.png %}
[19]: https://dashboard-01.braze.com/app_settings/app_settings
[20]: #replace-xcode-project
[21]: #append-to-xcode-project
[25]: {% image_buster /assets/img_archive/unity-ios-iam-listener.png %}
[26]: #manual-iam-integration
[27]: https://github.com/Appboy/unity-sdk/blob/develop/Assets/Plugins/Appboy/Tests/AppboyBindingTester.cs#L14
[28]: #manual-feed-integration
[29]: #manual-content-cards-integration
[30]: {% image_buster /assets/img_archive/unity-ios-feed-listener.png %}
[34]: {{site.baseurl}}/developer_guide/platform_integration_guides/unity/in-app_messaging/#in-app-messaging
[35]: {{site.baseurl}}/developer_guide/platform_integration_guides/unity/x_news_feed/#news-feed
[36]: https://github.com/Appboy/unity-sdk/blob/develop/Assets/Plugins/Appboy/Tests/AppboyBindingTester.cs#L56
[37]: {{site.baseurl}}/developer_guide/platform_integration_guides/unity/z_advanced_use_cases/customizing_the_unity_package/#customizing-the-unity-package
[38]: https://github.com/Appboy/appboy-unity-sdk/blob/master/Assets/Plugins/Appboy/Tests/AppboyBindingTester.cs
[39]: https://github.com/Appboy/appboy-unity-sdk/blob/master/Assets/Plugins/Appboy/models/Cards/ContentCard.cs
[40]: {% image_buster /assets/img_archive/unity-ios-content-cards-listener.png %}
[41]: https://docs.unity3d.com/Manual/AssetPackages.html
[42]: https://github.com/Appboy/appboy-unity-sdk/releases
