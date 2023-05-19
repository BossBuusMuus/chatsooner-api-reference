# Campaigns

We bring you the ability to run super charged⚡ WhatsApp campaigns through chatinc's Campaigns API. 

This API allows developers to focus solely on the conversational experience they wish to provide their users without having to focus on WhatsApp's core API or ever be exposed to it, allowing faster development cycles and happier teams😄  

## Campaigns in general

> Use this endpoint to create a campaign

```javascript

chat.createCampaign({
  title: "My Campaign",
  message: "Hello, World.",
  messageExample: "Hello, World.",
  buttons: [
    {
      text: "Start",
      type: "journey",
      journeyId: "myJourneyId"
    }
  ]
}, {
  runImmediately: true,
  audiences: ["myTag"],
  whatsAppTemplateType: "MARKETING" | "UTILITY" | "AUTHENTICATION"
}).then((response) => {
  console.log(response);
}).catch((error) => {
  console.log(error);
});
```

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
      "type" : "journey",
			"journeyId": "myJourneyID"
		},
    {
      "text": "Vist site",
      "type" : "url",
      "url": "https://www.google.com"
    }
	],
}'

```
**Campaigns** involve sending informative broadcasts to users, expecting them to take certain actions and receiving certain feedback. There's a lot you can do with the Campaigns API. By combining a few special components together mainly the Campaigns API itself, [Bot Builder](#journeys) APIs, and [Catalog](#catalog) APIs you can generate any number of scenarios for your campaigns.

### Creating a Campaign

`POST https://api.chatinc.com/v1/create/campaign`

This endpoint allows you to create a campaign at any time. A campaign is a message that you send to your users. You can specify the message, the buttons, and the audience you want to send the campaign to. 

### Post Parameters

Parameter | Required | Description
--------- | ------- | -----------
title | yes | This is the name of the campaign. You will be able to identify your campaign by the title you give it.
message | yes | This will be the display text that users read from your campaign. You can specifiy where variables will be placed through the `{variable_group_name.variable_name}` syntax. See the [Messages with Variables](#messages-with-variables) section to learn more about variables.
imgUrl |optional | An image for your campaign
options.whatsAppTemplateType | optional | Defaults to  `MARKETING` if not specified. Available types `MARKETING` | `UTILITY` | `AUTHENTICATION`
buttons | reccommended | Buttons are the actions your users will take after receiving your campaign message. Buttons is an `Array` with, for example, `{text:"Hello", "type":"journey","journeyId:"journeyId"}` objects. The journeyId is created from the builder. See the [builder api](#bot-builder) section to learn more about creating journeys and retrieving journey IDS. You can also create button of type phone, url or input. See the [Buttons](#buttons) section to learn more about buttons.
options.runImmediately | optional | If set to `true` the campaign will be sent immediately. Defaults to `false` if not specified.
options.audiences | optional | An array of audiences that you want to target with your campaign. Use the id `all`. See the [Contacts API](#contacts) section to learn more about creating audiences and retrieving audience tags.
options.languageCode | optional | The language code of the campaign. Defaults to `enUS` if not specified.
options.timeToRun | optional | Unix timestamp of the time you want the campaign to run. If not specified, the campaign will run immediately.


## Buttons

There are 4 types of buttons you can create for your campaign.

### Journey Buttons

Journey buttons are buttons that take your users to a journey. A journey is a series of messages that you create using the [Bot Builder](#bot-builder) API. You can create a journey button like so:

```javascript
{
  text: "Start",
  type: "journey",
  journeyId: "myJourneyId"
}
```

### Phone Buttons

Phone buttons are buttons that allow your users to call a phone number. You can create a phone button like so:

```javascript
{
  text: "Call Us",
  type: "phone",
  phoneNumber: "+1234567890"
}
```

### URL Buttons

URL buttons are buttons that allow your users to visit a URL. You can create a URL button like so:

```javascript
{
  text: "Visit Site",
  type: "url",
  url: "https://www.google.com"
}
```

### Input Buttons

Input buttons are buttons that allow your users to send a message to your bot. Input buttons collect the simple text that your users send and add them to an [Input Group](#input-groups) with the same name as the campaign. You can create an input button like so:

```javascript
{
  text: "Send Message",
  type: "input",
  input: "Hello, World."
}
```



### Running a Campaign

> Use this endpoint to create a campaign

```javascript

chat.runCampaign({
  campaignId: "myCampaignId",
  audiences:[
    "all"
  ]
}).then((response) => {
  console.log(response);
}).catch((error) => {
  console.log(error);
});
```

```shell
curl --request POST \
  --url https://api.chatinc.com/v1/create/campaign \
  --header 'Authorization: [API Key]' \
  --header 'Content-Type: application/json' \
  --data '{ "campaignId": "myCampaignId" }'
```


`POST https://api.chatinc.com/v1/run/campaign`

This endpoint allows you to run a campaign that you have already created. This is useful if you had set the `runImmediately` parameter to `false` when creating the campaign and you want to run it at your own time.

### Post Parameters

Parameter | Required | Description
--------- | ------- | -----------
campaignId | yes | The ID of the campaign you want to run. You can retrieve the campaign ID from the [Campaigns API](#campaigns)
audiences | yes | An array of audiences, specified by audienceIds, that you want to target with your campaign. If you wish to target all your contacts use the id `all`. See the [Contacts API](#contacts) section to learn more about creating audiences and retrieving audience Ids
timeToRun | optional | The time you want to run the campaign. This is a UNIX timestamp. If you don't specify a time, the campaign will be run immediately.

### Messages with Variables

Variables are a special component of the Campaigns API. They allow you to dynamically change the message that is sent to your users.

### Variable Groups

Variable groups are a group of variables that are related to each other. For example, if you want to send a message to a user that says "Hello, John Doe. How are you today?" you would create a variable group named `users` and add two variables to it, `first_name` and `last_name`. You would then use the variable group in your message like so: `Hello, {users.first_name} {users.last_name}. How are you today?`

To create variables groups you can use the [Variable Groups API](#create-variable-group).
### Retrieving variable groups

> Use this endpoint to retrieve all your variable groups

```javascript

chat.retrieveVariableGroups().then((response) => {
  console.log(response);
}).catch((error) => {
  console.log(error);
});
```

```shell

curl --request POST \
  --url https://api.chatinc.com/v1/retrieve/variable/groups \
  --header 'Authorization: [API Key]' \
  --header 'Content-Type: application/json' \
```

> The above endpoint returns an object structured as below:

```json
[
  {
    "name": "users",
     "fields": [
      "first_name",
      "last_name"
     ]
    
  }
]
```
When you retrieve variable groups you will get an array of objects. Each object will have a `name` and `fields` property. The `name` property is the name of the variable group. The `fields` property is an array of the variables in the variable group. See the [Variable Groups API](#create-variable-group) section to learn more about retrieving variable groups.

You will also receive variable groups that have been created as a result of creating input groups. See the [Input Groups](#input-groups) section to learn more about input groups.















## Data From Campaigns

Being able to see the results from your Campaign is a huge requirement of yours and your organisation's. We've tried to make the data returned from Camapigns to be as comprehensive as possible. 

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

