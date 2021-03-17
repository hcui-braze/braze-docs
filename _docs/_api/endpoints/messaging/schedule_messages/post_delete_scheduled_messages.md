---
nav_title: "POST: Delete Scheduled Messages"
page_order: 4

layout: api_page

page_type: reference
platform: API
tool:
  - Canvas
  - Campaigns

description: "This article outlines details about the Delete Scheduled Messages Braze endpoint."
---
{% api %}
# Delete Scheduled Messages
{% apimethod post core_endpoint|https://www.braze.com/docs/core_endpoints %} 
/messages/schedule/delete
{% endapimethod %}

The delete scheduled messages endpoint allows you to cancel a message that you previously scheduled _before_ it has been sent.

{% apiref postman %}https://documenter.getpostman.com/view/4689407/SVYrsdsG?version=latest#5e89355c-0a5d-4d8b-8d89-2fd99bac36b0 {% endapiref %}

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
  "schedule_id": (required, string) the schedule_id to delete (obtained from the response to create schedule)
}
```

### Request Parameters

| Parameter | Required | Data Type | Description |
| --------- | ---------| --------- | ----------- |
| `schedule_id` | Required | String | The schedule_id to delete (obtained from the response to create schedule) |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3  .reset-td-br-4}

## Request Components
- [Campaign Identifier]({{site.baseurl}}/api/identifier_types/)

### Example Request
```
curl --location --request POST 'https://rest.iad-01.braze.com/messages/schedule/delete' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_REST_API_KEY' \
--data-raw '{
  "schedule_id": "248762133332"
}
'
```

{% endapi %}
