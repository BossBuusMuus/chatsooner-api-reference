# Bot Builder

To have a commanding knowledge of how to build  **Conversational Experiences** on Chat Inc. You have to
learn about *2 key concepts*, [`Forms`](#forms) and [`Flows`](#flows).

<aside class="notice">
We have a Visual Bot Builder💡 that makes the work of building out your conversational experiences more seamless. While the below references contain information petaining the API, we recommend you understand them to be a wiz!🤸🏾‍♀️⚡
</aside>

## Forms

> The form object looks as follows

```json
{ 
  
  "type":"",
  "id":"",
  "name":"",
  "entry":"",
  "message":"",
  "exit":{},
  "options":[],
  "questions":[],
  "header":"",
  "innerTitle":"",
  "imageUrl":""
}
```

A *form*, is a message. When displaying a message you want to configure *what* the message says, *where* or *when* it shows up, and *how* it shows up.

Below is a table showing the most necessary fields available on the form object regardless of its type.

Field | Required | Description
--------- | ------- | -----------
type | yes | The form type. See form [types](#form-types) to learn more about the values you can specifiy here.
id | yes | A unique identifier for this form.
name | yes | A Human readable name for the form. **Note** this value is displayed to the user if the form shows menu options. This value does not need to be unique.
entry | yes | A second unique identifier for the form. The entry is meant to be a human readable identifier for quick debugging. A good practice is to combine the **name** and **id** as a value for this field.
message | yes | The text displayed to the user when this form is active.
options | yes | This an array of values to present to the user as button options. **Note:** This field is only required for the **agent.selection** form type that shows buttons
exit | yes | The exit field specifies where the form points out to after a user is done interacting with it. Intuitively, the exit field points to other forms.
tag | optional | **Object**. When you specifiy this field a tag value is then attributted to a user when this form is active. Tags are useful in segmenting your chats and are strongly tied to [tasks](#tasks).

### More on the exit field

> The structure of the exit field is as follows:

```json
{
  "option1":"form.entry1",
  "option2": "form.entry2"
}
```

 The form that will ultimately be activated after the current one depends on the user input. User input can either be free text or button options. The `exit` field is an object with `option` and `entry` key-value pairs.  The option key should be a value found in the `options` array of the form, whereas the entry value should be the `entry` value of a form you have already created. You should specify as many `option` and `entry` key-value pairs as there are options on your form.



## Form Types

You would want to display different types of forms and for that we have various form types.

### agent.selection

> The object for the form type agent.selection looks as follows:

```json

{ 
  
  "type":"agent.selection",
  "id":"myId",
  "name":"Welcome",
  "entry":"myId-Welcome",
  "message":"Hi this is a message with button options",
  "options":["Nice", "Okay"] or [{
    "title":"Nice",
    "description":"This is a list description"
  }, {
    "title":"Okay",
    "description":"This is a list description"
  }],
  "exit":{
    "Okay":"myId2-OkayForm",
    "Nice":"myId3-NiceForm"
  },
  "innerTitle":"Choose Below",
  "header":"Options",
 
}
```

This form type is for displaying messages with buttons.

Below are additional fields you can specify to further customize your message with buttons.

Field | Required | Description
--------- | ------- | -----------
options | true | A array of the buttons that will be shown to the user. It is acceptable to have this field as an array of objects with **title** and **description** fields to allow for menu lists with descriptions.
header | optional | The header of the message
innerTitle | optional | The header of the menu list.

### agent.text

> The object for the form type agent.text looks as follows

```json

{ 
  
  "type":"agent.text",
  "id":"myId",
  "name":"Welcome",
  "entry":"myId-Welcome",
  "message":"Hi this is a message with button options",
  "exit":"myForm.entry",
}

```

This form is used for displaying plain messages. Note that for the `exit` field we specify a 
string and not an array.

Please note the below  additional specifications for this form's fields.


Field | Required | Description
--------- | ------- | -----------
exit | true | The exit for this form type should be a string because we do not need a mapping of options to a corresponding entry. This form exits to only one other form.

### agent.survey

> The object for the form type agent.survey looks as follows

```json
{
  { 
  
  "type":"agent.text",
  "id":"myId",
  "name":"Welcome",
  "entry":"myId-Welcome",
  "message":"Hi this is a message with button options",
  "exit":"myForm.entry",
  "questions":[
    {
      "id": 1,
      "name":"Email Address",
      "options":[],
      "message":"What is your email address?",
      "validate":"email",
    },
     {
      "id": 2,
      "name":"Continent",
      "options":["Africa", "South America", "Europe"],
      "message":"On which continent do you reside",
      "validate":"email",
    }
  ]
}
}
```

This form is used when you wish to collect input from a user, usually through a series of questions.
While this form displays multiple messages to users it is infact still one form.

Please note the below additional specifications for this form's fields.

Field | Required | Description
--------- | ------- | -----------
questions | true | An array of the questions that will be displayed to the user. See more information about the survey question object below.
exit | true | The exit for this form type should be a string because we do not need a mapping of options to a corresponding entry. This form exits to only one other form.

### The Survey Question Object

> The structure of the survey question object is as follows:

```json
   {
      "id": 1,
      "name":"Email Address",
      "options":[],
      "message":"What is your email address?",
      "validate":"email",
    }
```

This object represents a question that will be asked to the user.


Field | Required | Description
--------- | ------- | -----------
id | true | This is a unique identifier for the question. It should always be an integer. It also denotes when the question will show up in the series of questions. 1 denotes this is the first question. Note, should this specification not be followed an error will be reported.
name | true | This is the identifier denoted to this questions answer.
message | true | The message diplayed to the user.
options | optional | An array of the options to display to the user.
validate | optional | Here you can specifiy validation on the input added by the user. The following validation types are available **email**, **address**, **number**.

## Flows

> Use this request to create a flow

```shell
curl --request POST \
  --url https://api.chatinc.com/create/flow \
  --header 'Authorization: [API Key}' \
  --header 'Content-Type: application/json' \
  --data '{
	"flowId": "myFlowId",
	"entry": "myFormEntry",
	"forms": [
		{
			"type": "agent.text",
			"id": "myId",
			"name": "Welcome",
			"entry": "myId-Welcome",
			"message": "Hi this is a message with button options",
			"exit": "myForm.entry",
			"questions": [
				{
					"id": 1,
					"name": "Email Address",
					"options": [],
					"message": "What is your email address?",
					"validate": "email"
				},
				{
					"id": 2,
					"name": "Continent",
					"options": [
						"Africa",
						"South America",
						"Europe"
					],
					"message": "On which continent do you reside",
					"validate": "email"
				}
			]
		},
		{ 
			"type":"agent.text",
			"id":"myId",
			"name":"Thank You",
			"entry":"myId-Thank You",
			"message":"Thanks for filling out our form",
			"exit":""
		}
	]
}'

```

**Flows** a.k.a sequences are just a series of forms stitched together to create a uniquely identifiable user journey.

**Note:** We do not use tree structures but rather the `entry` and `exit` convention to connect forms together.

### HTTPS Request

`POST https://api.chatinc.com/create/flow`

### POST Parameters


Field | Required | Description
--------- | ------- | -----------
flowId | true | A unique identifier for this flow
entry | true | This the entry value of the form that will be the first to be called in this flow. Entry can also be a keyword. If the entry value does not match to any form, this value will be treated as a key word.
forms |true | An array of form objects.

### Retrieving flows

> Call this request to retrieve flows:

```shell
curl --request GET \
  --url https://api.chatinc.com/v1/retrieve/flows\
  --header 'Authorization: [API Key]' \
```

> Calling this request returns a json object structured as follows:

```json

[
  {
	"flowId": "Welcome",
	"entry": "welcome",
	"forms": [
		{ 
			"type":"agent.text",
			"id":"myId",
			"name":"Welcome",
			"entry":"myId-welcome",
			"message":"Hey there welcome to your chatbot",
			"exit":""
		}
	]
}
]


```

### HTTPS Request

`GET https://api.chatinc.com/retrieve/flows`

Even when you haven't created a flow you'll always receive the **Welcome** Flow because it is a system created flow. The **Welcome** is the default flow and is activated when a user says *Hi*.


### Editing flows

> Use this endpoint to edit a form or forms within a flow.

```shell
curl --request POST \
  --url https://api.chatinc.com/edit/flow \
  --header 'Authorization: [API Key]' \
  --header 'Content-Type: application/json' \
  --data '{
	"flowId": "Welcome",
	"entry": "welcome",
	"forms": [
		{ 
			"type":"agent.text",
			"id":"myId",
			"name":"Welcome",
			"entry":"myId-welcome",
			"message":"Hey there welcome to your chatbot. Today!",
			"exit":""
		}
	]
}'
```

To edit a flow you mainly want to edit existing forms within the flow or add new ones. To edit existing forms within a flow you want to change certain fields within a form but not the form's `id` or `entry`. So just keep the `id` and `entry` the same and change the fields you wish. To add new forms just add the forms you wish to add within the request.

You can change the `entry` for the flow by specifying new values as well.

### Post Parameters

Field | Required | Description
--------- | ------- | -----------
flowId | true | A unique identifier for this flow
entry | optional | This the entry value of the form that will be the first to be called in this flow. Entry can also be a keyword. If the entry value does not match to any form, this value will be treated as a key word.
forms |optional | An array of form objects.


## Tasks

> Use this request to create a task group

```shell
  curl --request POST \
  --url https://api.chatinc.com/create/task/group \
  --header 'Authorization: [API Key]' \
  --header 'Content-Type: application/json' \
  --data '{
	"taskId": "myTaskId",
	"group": [
		{
			"tag": "Remind Me Later",
			"overridingTag": "RSVP",
			"type": "flow",
			"typeId": "myFlowId",
			"runAt": "",
			"runAfter": ""
		}
	]
}'
```

Tasks are a special way to perform timed activities resulting in prompting a user. For example you might want to schedule when to send a campaign message or you might want to prompt a user who didn't complete a form or you might want to run a series of surveys at different times but resulting as the same campaign. 

### Tags

You may have noticed tags when we were talking about [forms](#forms). Tags are attributes given to a chat when they interact with a specific form. For example a user who clicks a "Remind Me Later** button and goes on to see a *"Alright we will remind you later"* message will have to be tagged so that we know to perform that specific task. 

### The Task Group

You often want a *series of tasks* as a single task. Because of this we naturaly add tasks as a group. 

Parameter | Required | Description
--------- | ------- | -----------
taskGroupId | yes | **String**: A unique identifier for this task group
group | yes | **Array**: An array of **Task** objects

### Task Object

This object represents the actual task to be run.

Parameter | Required | Description
--------- | ------- | -----------
tag | yes | **String**. The name of the tag that a chat object should have for this task to be run.
type | yes | **String**. One of three supported types: "template", "flow" or "message"
typeId | yes | **String**. The id of the "template", "flow" or "message" that will be sent as payload for this task.
runAt | optional | The precise datetime of when this task should run.
runAfter | optional | **String**. Value in minutes after inactivity from the user when this task should run.
overridingTag | recommended | It is also almost always the case that a users interaction with your channel will change wether or not a tag added prior should still be present on the chat. As such you can specify a tag which once is added to the user would render a prior tag obsolete. See [recipes](#recipes) to see this in action.





















