---
nav_title: "POST: Create New User Alias"
page_order: 4

layout: api_page

page_type: reference
platform: API
tool:
  - Canvas
  - Campaigns

description: "This article outlines details about the create new User Aliases Braze endpoint."
---
{% api %}
# Create New User Alias
{% apimethod post %}
/users/alias/new
{% endapimethod %}

Use this endpoint to add new user aliases for existing identified users, or to create new unidentified users.

Adding a user alias for an existing user requires an `external_id` to be included in the new user alias object. If the `external_id` is present in the object but there is no user with that `external_id`, the alias will not be added to any users.

Creating a new alias-only user requires the `external_id` to be omitted from the new user alias object. Once the user is created, use the `/users/track` endpoint to associate the alias-only user with attributes, events and purchases, and the `/users/identify` endpoint to identify the user with an `external_id`.

You can add up to 50 user aliases per request.

{% apiref swagger %}https://www.braze.com/docs/api/interactive/#/User%20Data/NewUserAliasRequestExample {% endapiref %}
{% apiref postman %}https://documenter.getpostman.com/view/4689407/SVYrsdsG?version=latest#5cf18e64-fd02-452f-8c90-9a0f7c4d0487 {% endapiref %}

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
   "user_aliases" : (required, array of New User Alias Object)
}
```

### Request Parameters

| Parameter | Required | Data Type | Description |
| --------- | ---------| --------- | ----------- |
| `user_aliases` | Yes | Array of New User Alias Objects | See New User Alias Object |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3  .reset-td-br-4}

### Request Components
- [User Alias Object]({{site.baseurl}}/api/objects_filters/user_alias_object/)
<br><br>
For more information on `alias_name` and `alias_label`, check out our [User Aliases documentation]({{site.baseurl}}/user_guide/data_and_analytics/user_data_collection/user_profile_lifecycle/#user-aliases)


###  Endpoint Request Body with New User Alias Object Specification

```json
{
  "external_id" : (optional, string) see External User ID below,
  "alias_name" : (required, string),
  "alias_label" : (required, string)
}
```

If an `external_id` is not present, a user will still be created, but will need to be identified later. You can do this using the "Identifying Users" and the `users/identify` endpoint.

### Example Request
```
curl --location --request POST 'https://rest.iad-01.braze.com/users/alias/new' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_REST_API_KEY' \
--data-raw '{
  "user_aliases" : 
  [
    {
      "external_id": "user_id",
      "alias_name" : "name",
      "alias_label" : "label"
    }
  ]
}'
```

{% endapi %}

