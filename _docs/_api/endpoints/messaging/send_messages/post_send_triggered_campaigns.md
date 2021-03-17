---
nav_title: "POST: Send Campaign Messages via API Triggered Delivery"
page_order: 4

layout: api_page

page_type: reference
platform: API
tool:
  - Campaigns

description: "This article outlines details about the Send Campaign Messages via API Triggered Delivery Braze endpoint."
---
{% api %}
# Sending Campaign Messages via API Triggered Delivery
{% apimethod post core_endpoint|https://www.braze.com/docs/core_endpoints %} 
/campaigns/trigger/send
{% endapimethod %}

API Triggered Delivery allows you to house message content inside of the Braze dashboard while dictating when a message is sent, and to whom via your API.

The send endpoint allows you to send immediate, ad-hoc messages to designated users. If you are targeting a segment, a record of your request will be stored in the [Developer Console](https://dashboard.braze.com/app_settings/developer_console/activitylog/).

This endpoint allows you to send Campaign messages via API Triggered delivery, allowing you to decide what action should trigger the message to be sent. Please note that to send messages with this endpoint, you must have a Campaign ID, created when you build an [API Triggered Campaign]({{site.baseurl}}/api/api_campaigns/).

{% apiref swagger %}https://www.braze.com/docs/api/interactive/#/Messaging/SendApiTriggeredDeliveryCampaignExample {% endapiref %}
{% apiref postman %}https://documenter.getpostman.com/view/4689407/SVYrsdsG?version=latest#aef185ae-f591-452a-93a9-61d4bc023b05 {% endapiref %}

{% alert important %}
__Looking for the `api_key` parameter?__<br>As of May 2020, Braze has changed how we read API keys to be more secure. Now API keys must be passed as a request header, please see `YOUR_REST_API_KEY` within the __Example Request__ below.<br><br>Braze will continue to support the `api_key` being passed through the request body and URL parameters, but will eventually be sunset. Please update your API calls accordingly.
{% endalert %}

## Request Body

```
Content-Type: application/json
Authorization: Bearer YOUR_REST_API_KEY
```

```json
{
  "campaign_id": (required, string) see Campaign Identifier,
  "send_id": (optional, string) see Send Identifier,
  "trigger_properties": (optional, object) personalization key value pairs that will apply to all users in this request,
  "broadcast": (optional, boolean) see Broadcast -- defaults to false on 8/31/17, must be set to true if "recipients" is omitted,
  "audience": (optional, Connected Audience Object) see Connected Audience,
  // Including 'audience' will only send to users in the audience
  "recipients": (optional, array; if not provided and broadcast is not set to 'false', message will send to the entire segment targeted by the campaign) [
    {
      // Either "external_user_id" or "user_alias" is required. Requests must specify only one.
      "user_alias": (optional, User Alias Object) User Alias of user to receive message,
      "external_user_id": (optional, string) External Id of user to receive message,
      "trigger_properties": (optional, object) personalization key value pairs that will apply to this user (these key value pairs will override any keys that conflict with trigger_properties above),
      "send_to_existing_only": (optional, boolean) defaults to true, if set to `false`, an attributes object must also be included,
      "attributes": (optional, object) fields in the attributes object will create or update an attribute of that name with the given value on the specified user profile before the message is sent and existing values will be overwritten
    },
  ]
}
```

### Request Parameters

| Parameter | Required | Data Type | Description |
| --------- | ---------| --------- | ----------- |
|`campaign_id`|Required|String|See Campaign Identifier|
|`send_id`| Optional | String | See Send Identifier |
|`trigger_properties`|Optional|Object|Personalization key value pairs that will apply to all users in this request|
|`broadcast`|Optional|Boolean|See Broadcast -- defaults to false on 8/31/17, must be set to true if "recipients" is omitted|
|`audience`|Optional|Connected Audience Object|See Connected Audience|
|`recipients`|Optional|Array|If not provided and broadcast is not set to 'false', message will send to the entire segment targeted by the Campaign|
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3  .reset-td-br-4}

### Request Components
- [Campaign Identifier]({{site.baseurl}}/api/identifier_types/)
- [Broadcast]({{site.baseurl}}/api/parameters/#broadcast)
- [Connected Audience]({{site.baseurl}}/api/objects_filters/connected_audience/)
- [Recipients]({{site.baseurl}}/api/objects_filters/recipient_object/)
- [User Alias Object]({{site.baseurl}}/api/objects_filters/user_alias_object/)
- [User Attributes Object]({{site.baseurl}}/api/objects_filters/user_attributes_object/)
- [API Parameters]({{site.baseurl}}/api/parameters)
<br><br>
The recipients array may contain up to 50 objects, with each object containing a single `external_user_id` string and `trigger_properties` object.
<br><br>
When `send_to_existing_only` is `true`, Braze will only send the message to existing users. When `send_to_existing_only` is `false` and a user with the given `id` does not exist, Braze will create a user with that id and attributes before sending the message.

### Example Request
```
curl --location --request POST 'https://rest.iad-01.braze.com/campaigns/trigger/send' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_REST_API_KEY' \
--data-raw '{
  "campaign_id": "",
  "send_id": "",
  "trigger_properties": {},
  "broadcast": false,
  "audience": {
    "AND": [
      {
        "custom_attribute": {
          "custom_attribute_name": "eye_color",
          "comparison": "equals",
          "value": "blue"
        }
      },
      {
        "custom_attribute": {
          "custom_attribute_name": "favorite_foods",
          "comparison": "includes_value",
          "value": "pizza"
        }
      },
      {
        "OR": [
          {
            "custom_attribute": {
              "custom_attribute_name": "last_purchase_time",
              "comparison": "less_than_x_days_ago",
              "value": 2
            }
          },
          {
            "push_subscription_status": {
              "comparison": "is",
              "value": "opted_in"
            }
          }
        ]
      },
      {
        "email_subscription_status": {
          "comparison": "is_not",
          "value": "subscribed"
        }
      },
      {
        "last_used_app": {
          "comparison": "after",
          "value": "2019-07-22T13:17:55+0000"
        }
      }
    ]
  },
  "recipients": [{
    "user_alias": {
      "alias_name" : "",
      "alias_label" : ""
    },
    "external_user_id": "",
    "trigger_properties": {},
    "send_to_existing_only": true,
    "attributes": {}
  }]
}
'
```

## Response Details
Message sending endpoint responses will include the message’s `dispatch_id` for reference back to the dispatch of the message. The `dispatch_id` is the id of the message dispatch (unique id for each ‘transmission’ sent from the Braze platform). For more information on `dispatch_id` checkout out our [documentation]({{site.baseurl}}/help/help_articles/data/dispatch_id/).

## Create Send Endpoint

__Using the Attributes Object in Campaigns__

Braze has a Messaging Object called `Attributes` that will allow you to add, create, or update attributes and values for a user before you send them an API Triggered Campaigns using the `campaign/trigger/send` endpoint as this API call will process the User Attributes object before it processes and sends the campaign. This helps minimize the risk of there being issues caused by [race conditions]({{site.baseurl}}/help/best_practices/race_conditions/). 

{% alert important %}
Looking for Create Send Endpoint for Canvases? Check out the documentation [here]({{site.baseurl}}/api/endpoints/messaging/send_messages/post_send_triggered_canvases/#create-send-endpoint).
{% endalert %}

{% endapi %}


