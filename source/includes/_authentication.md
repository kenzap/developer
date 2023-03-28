# Authentication

There are two types of authorization methods:

* API key authorization
* User token authentication

##API Key Authorization

```javascript
fetch("https://api-v1.kenzap.cloud/", {
  method: "post",
  headers: {
    "Accept': 'application/json",
    "Content-Type': 'application/json",
    "Authorization': 'Bearer your_api_key"
  }
})
```

API key authorization is designed for **backend** communication between servers. This type of method should not be used in frontend production sites.

All Kenzap Cloud API requests must contain an API key in a header that looks like the following:

`Authorization: Bearer your_api_key`

You can register a new API key under the [Kenzap Cloud Access](https://dashboard.kenzap.cloud/access/) dashboard.

<aside class="notice">
You must replace <code>your_api_key</code> with your API key.
</aside>

##User Token Authentication

```javascript
fetch("https://api-v1.kenzap.cloud/", {
  method: "post",
  headers: {
    "Accept": "application/json",
    "Content-Type": "application/json",
    "Kenzap-Token": "kenzap_user_token",
    "Authorization": "Bearer your_api_key"
  }
})
```

User token authorization is designed for applications that need to manipulate data from the **frontend**. For example, a user places an order to the E-commerce extension.

All Kenzap Cloud API requests must contain **API key** and Kenzap **user token** in a header that looks like the following:

`Authorization: Bearer your_api_key`

`Kenzap-Token: kenzap_user_token`

You can register a new API key under the [Kenzap Cloud Access](https://dashboard.kenzap.cloud/access/) dashboard. You can retrieve kenzap_user_token from kenzap_token cookie when the user is authorized to the Kenzap Cloud.

<aside class="notice">
You must replace <code>your_api_key</code> with your API key and <code>kenzap_token</code> with real user token.
</aside>

##API Key Parameters

* Access type
    * **Frontend** - frontend queries (requires [kenzap_user_token](#user-token-authentication)).
    * **Backend** - server side communication.
* Permissions
    * **Read only** - allow read queries only.
    * **Read &amp; write** - allow read and write queries.
* Isolated
    * **Yes** - access data generated by the same API key or user.
    * **No** - access any data including data with the isolated parameter set to "Yes".

<aside class="warning">
Never use <code>backend</code> API keys with <code>read &amp; write</code> permissions in the frontend production sites.
</aside>