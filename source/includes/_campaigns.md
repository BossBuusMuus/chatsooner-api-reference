# Campaigns

We bring you the ability to run super charged⚡ WhatsApp campaigns through chatinc's Campaigns API. 

This API allows developers to focus solely on the conversational experience they wish to provide their users without having to focus on WhatsApp's core API or ever be exposed to it, allowing faster development cycles and happier teams😄  

## Campaigns in general

> Use this endpoint to send a campaign

```shell
curl --request POST \
  --url https://api.chatinc.com/v1/create/campaign \
  --header 'Authorization: [API Key]' \
  --header 'Content-Type: application/json' \
  --data '{
	"title": "campaign title",
	"langauageCode": "enUS",
	"templateType": "MARKETING",
	"message": "My message {}",
	"messageExample": "My message 3",
	"imgUrl": "my-imageurl",
	"buttonType": "QUICK_REPLY",
	"buttons": [
		{
			"text": "Start",
			"flowId": "myFlowID"
		}
	],
}'

```
**Campaigns** involve sending informative broadcasts to users, expecting them to take certain actions and receiving certain feedback. There's a lot you can do with the Campaigns API. By combining a few special components together mainly the Campaigns API itself, [Bot Builder](#flows) APIs, and [Catalog](#catalog) APIs you can generate any number of possibilities for your campaigns.

### HTTPS Request

`POST https://api.chatinc.com/v1/create/campaign`

### Post Parameters

Parameter | Required | Description
--------- | ------- | -----------
title | yes | This is the name of the campaign. You will be able to identify your campaign by the title you give it.
message | yes | This will be the display text that users read from your campaign. You can specifiy where variables will be placed through the `{}` syntax.
messageExample | optional | Required if message contains variables. Use this to show what the message would look like if the variables where filled.
imgUrl |optional | An image for your campaign
whatsAppTemplateType | optional | Defaults to  `MARKETING` if not specified.
buttons | reccommended | Buttons are the actions your users will take after receiving your campaign message. Buttons is an `Array` with `{text:"Hello", flowId:"flowId"}` objects. The flowId is created from the builder. See the [builder](#bot-builder) section to learn more about creating flows and retrieving flow IDS. 

## Data From Campaigns

Being able to see the results from your Campaign is a huge requirement to you and your organisation. We've tried to make the data returned from Camapigns to be as comprehensive as possible. 

### Retrieve All Campaigns

>Use this endpoint to retrieve all your campaigns

```shell
curl --request GET \
  --url https://api.chatinc.com/v1/retrieve/campaigns \
  --header 'Authorization: [API Key]' \
```

> The above endpoint returns an onject structured as below:

```json
[
  {

  }
]
```

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
active | optional | If set to `true` the endpoint only retrieves active campaigns

### HTTPS Request

`GET https://api.chatinc.com/v1/retrieve/campaigns`

### Retrieving Campaign Details

>Use this endpoint to retrieve details about a particular campaign

```shell
curl --request GET \
  --url https://api.chatinc.com/v1/retrieve/campaign/?campaignId=myCampaignId \
  --header 'Authorization: [API Key]' \
```

> The above code returs a JSON objecta structured as follows:

```json
[
  {
    
  }
]
```

### HTTPS Request

`GET https://api.chatinc.com/v1/retrieve/campaign?campaignId=myCampaignId`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
campaignId | yes | The ID of the campaign

