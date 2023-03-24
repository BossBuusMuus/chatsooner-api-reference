# Chats

> Make this request to retrieve chats

```shell
  curl --request GET \
  --url http://api.chatinc.com/retrieve/chats \
  --header 'Authorization: [API Key]'
```

> This request returns a json obect structured as follows:

```json
  [
    {

    },
    ...
  ]

```

Chats are the conversations that your users have with your channel. Since the context is always conversational it is worth noting that each chat is a user, and to perform any operations on your users you work with the chat object.

### HTTPS Request

`GET http://api.chatinc.com/retrieve/chats`

### Query Parameters

You are able to specify filters for your request. The below parameters are available.


Parameter | Required | Description
--------- | ------- | -----------
abondonedCarts | optional | **Boolean** Set to true to receive chats with abondonedCarts only
orders | optional | **Boolean** Set to true to receive chats with orders only

###





