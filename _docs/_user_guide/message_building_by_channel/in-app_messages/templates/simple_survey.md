---
nav_title: Simple Survey
platform: Message_Building_and_Personalization
subplatform: In-App Messages
page_order: 1.5
hidden: true
description: "Collect user attributes, insights, and preferences to power your campaign strategy using new In-App Message Surveys."
---

# Simple Survey In-App Message

Use the new Simple Survey In-App Message template to collect user attributes, insights, and preferences that power your campaign strategy.

For example, ask users how they'd like to use your app, learn more about their personal preferences, or even ask about their satisfaction with a particular feature.

![examples]({% image_buster /assets/img/iam/iam-survey.png %})

{% alert important %}
This feature is in a closed _Early Access_ period. To request access, please submit your feedback in our [Product Portal](https://dashboard.braze.com/resources/roadmap).
{% endalert %}

## SDK Requirements {#supported-sdk-versions}

This In-App Message will only be delivered to devices that use at least the [SDK versions]({{ site.baseurl }}/user_guide/engagement_tools/campaigns/ideas_and_strategies/new_features/#filtering-by-most-recent-app-versions) below:

{% sdk_min_versions web:2.5.0 android:8.0.0 ios:3.23.0 %}

## Creating a new Survey {#create}

When creating a new In-App message, choose the new "Simple Survey" option located in the Template Type menu.

![Simple Survey Message Type]({% image_buster /assets/img/iam/survey-message-type.png %}){: style="max-width:700px"}

This Survey template is supported on mobile, web, or both mobile + web. Remember to be sure that your SDKs are on the [minimum versions](#supported-sdk-versions) required for this feature.

### Customization Options {#options}

#### Single vs. Multiple Choice Survey {#single-multiple-choice}

Use the Single vs. Multiple Choice option to control whether a user can select only one choice or multiple choices.

When Custom Attribute collection is enabled, the Multiple Choice option will set each choice's designated custom attribute. So, be sure to use a unique Custom Attribute name for each choice to prevent choices from overwriting each other.

![Single Multiple Choice]({% image_buster /assets/img/iam/single-multiple-choice.png %}){: style="max-width:700px"}

### Collect Custom Attributes

Enable Custom Attribute collection to collect attributes based on the user's submission. Use this option to create new segments and retargeting campaigns. For example, in a satisfaction survey, you could send a follow-up email to all users who were not happy.

![Custom Attributes]({% image_buster /assets/img/iam/collect-attributes.png %}){: style="max-width:700px"}

### Showing A Confirmation Page

Once a user submits their response, you can optionally show a confirmation page, or, simply close the message.

A confirmation page is a great place to thank users for their time or provide additional information. You can customize the Call To Action on this page to guide users to another page of your app or website.

![Confirmation Option]({% image_buster /assets/img/iam/confirmation-option.png %}){: style="max-width:700px"}

![Confirmation Page]({% image_buster /assets/img/iam/confirmation-page.png %}){: style="max-width:700px"}

### Styling Your Message

Customize the font and accent color of the message using the Color Theme picker.

![Color Theme Picker]({% image_buster /assets/img/iam/color-theme-picker.png %}){: style="max-width:700px"}

## Analyzing Results

Analyze results in real-time in your message's analytics report to see how many users selected each choice.

![Analytics]({% image_buster /assets/img/iam/analytics.png %}){: style="max-width:700px"}

 <!-- Retargeting Using Custom Attributes -->
