---
nav_title: Inbox Vision
platform: Message_Building_and_Personalization
subplatform: Email
page_order: 5
description: "Inbox Vision allows marketers to view their emails from the perspective of various email clients and mobile devices. This reference article covers how to set up and use Inbox Vision."
---

# Inbox Vision

Inbox Vision allows marketers to view their emails from the perspective of various email clients and mobile devices. Access Inbox Vision from the email editor by clicking the `Preview and Test` button.  It also allows you to spam test from the Spam Test tab.

## Test Your Email

To test your email message in Inbox Vision, click `Preview and Test` within the email composer. Braze then sends an HTML version of your email to various email clients used across the globe, which may take between two and ten minutes to complete.

Braze will then display screenshots of a sample, rendered HTML on desktops, mobile devices, and tablets. The devices in which screenshots are displayed are scrollable, to allow for better viewing.

If you run an Inbox Vision test, you will also receive a code analysis and spam testing results.

![inboxvision1][1]

{% alert important %}
Once you make changes to a template, you will need to re-run the test to see the effect of the changes on the previews.
{% endalert %}

## Code Analysis

Code analysis is a way for Braze to highlight issues that may exist with your HTML, showing the number of occurrences of each issue and providing insight into which HTML elements are not supported. This information found on the Inbox Vision preview page by selecting the list icon in the upper left-hand corner.

![inboxvision2][2]
![inboxvision3][3]


## Spam Testing

[Spam testing][4] attempts to predict whether your email will land in spam folders or in your customers' inboxes.  Spam testing runs across major spam filters, such as IronPort, SpamAssassin, and Barracuda, as well as major ISP filters such as Gmail.com and Outlook.com.

### FAQ

{% details What does the reprocess screenshot button do? %}
Very rarely, you will encounter that some screenshots for certain email clients are not clear.  The reprocess screenshot button will create another screenshot.
{% enddetails %}

{% details Why does the email preview not appear but my code analysis does for the same client? %}
Taking a screenshot takes longer than code analysis since we wait till the email arrives in the inbox before taking the screenshot. As a result, sometimes the code analysis will show up faster than the preview for a particular email client.
{% enddetails %}

{% details Can I trust the accuracy of your email test results? %}
All of our tests are run through actual email clients. We work hard to ensure that all renderings are as accurate as possible.  If you consistently see an issue with an email client, please [open a support ticket]({{site.baseurl}}/support_contact/).
{% enddetails %}

{% details Why is my email is not rendering? %}
In general, if your email content relies on templating info such as user profile information, it will not work with Inbox Vision. This is because Braze templates in an empty user when we send emails using this feature.
{% enddetails %}


[1]: {% image_buster /assets/img_archive/inboxvision1.png %}
[2]: {% image_buster /assets/img_archive/inboxvision2.png %}
[3]: {% image_buster /assets/img_archive/inboxvision3.png %}
[4]: {{site.baseurl}}/email_spam_testing/
