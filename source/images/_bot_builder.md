# Bot Builder

To have a commanding knowledge of how to build  **Conversational Experiences** on Chat Inc. you have to
learn about *2 key concepts*, [`Steps`](#steps) and [`Journeys`](#journeys).

<aside class="notice">
We have a Visual Bot Builder💡 that makes the work of building out your conversational journeys more seamless.🤸🏾‍♀️⚡
</aside>

## Steps

> The step object looks as follows

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

A *step*, is a message a user interacts with at any given time. When displaying a message you want to configure *what* the message says, *where* or *when* it shows up, and *how* it shows up.

Below is a table showing the most necessary fields available on the step object regardless of its type.

Field | Required | Description
--------- | ------- | -----------
type | yes | The step type. See step [types](#step-types) to learn more about the values you can specifiy here.
id | yes | A unique identifier for this step.
name | yes | A Human readable name for the step. **Note** this value is displayed to the user if the step shows menu options. This value does not need to be unique.
entry | yes | A second unique identifier for the step. The entry is meant to be a human readable identifier for quick debugging. A good practice is to combine the **name** and **id** as a value for this field.
message | yes | The text displayed to the user when this step is active.
options | yes | This an array of values to present to the user as button options. **Note:** This field is only required for the [selection](#selection) step type that shows button or list options to the user.
exit | yes | The exit field specifies where the step points out to after a user is done interacting with it. Intuitively, the exit field points to other steps.
tag | optional | **Object**. When you specifiy this field a tag value is then attributted to a user when this step is active. Tags are useful in segmenting your chats and are strongly tied to [tasks](#tasks).

### More on the exit field

> The structure of the exit field is as follows:

```json
{
  "option1":"step.entry1",
  "option2": "step.entry2"
}
```

 The step that will ultimately be activated after the current one depends on the user input. User input can either be free text or button\list options. The `exit` field is an object with `option` and `entry` key-value pairs.  The `key` should be a value found in the `options` array of the step and the `value` an `entry` of the step that should be activated when a user clicks that option. You should always make sure that the values in the option array are the `entry` values of steps you have created or that already exist. There should be as many key-value pairs in the exit object as there are options on the step.



## Step Types

You would want to display different types of steps and for that we have various step types.

### selection

> The object for the step type selection looks as follows:

```json

{ 
  
  "type":"selection",
  "id":"myId",
  "name":"Welcome",
  "entry":"myId-Welcome",
  "message":"Hi this is a message with button options",
  "options": [{
    "title":"Nice",
    "description":"This is a list description"
  }, {
    "title":"Okay",
    "description":"This is a list description"
  }],
  "exit":{
    "Okay":"myId2-OkayStep",
    "Nice":"myId3-NiceStep"
  },
  "innerTitle":"Choose Below",
  "header":"Options",
 
}
```

This step type is for displaying messages with buttons.

Below are additional fields you can specify to further customize your message with buttons or a list.

Field | Required | Description
--------- | ------- | -----------
options | true | A array of the values that will be shown to the user as buttons or in a list. You should specify these values as objects with **title** and **description** fields to allow for menu lists with descriptions.
header | optional | The header of the message
innerTitle | optional | The header of the menu list.

### text

> The object for the step type text looks as follows

```json

{ 
  
  "type":"text",
  "id":"myId",
  "name":"Welcome",
  "entry":"myId-Welcome",
  "message":"Hi this is a message with button options",
  "exit":"myStep.entry",
}

```

This step is used for displaying plain steps. Note that for the `exit` field we specify a string and not an array.

Please note the below additional specifications for this step's fields.


Field | Required | Description
--------- | ------- | -----------
exit | true | The exit for this step type should be a string because we do not need a mapping of options to a corresponding entry. This step exits to only one other step.

### survey

> The object for the step type survey looks as follows

```json
{
  { 
  
  "type":"text",
  "id":"myId",
  "name":"Welcome",
  "entry":"myId-Welcome",
  "message":"Hi this is a message with button options",
  "exit":"myStep.entry",
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

This step is used when you wish to collect input from a user, usually through a series of questions.
While this step displays multiple messages to users it is infact still one step.

Please note the below additional specifications for this step's fields.

Field | Required | Description
--------- | ------- | -----------
questions | true | An array of the questions that will be displayed to the user. See more information about the survey question object below.
exit | true | The exit for this step type should be a string because we do not need a mapping of options to a corresponding entry. This step exits to only one other step.

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

## Journeys

> Use this request to create a journey

```shell
curl --request POST \
  --url https://api.chatinc.com/create/journey \
  --header 'Authorization: [API Key}' \
  --header 'Content-Type: application/json' \
  --data '{
	"journeyId": "myJourneyId",
	"entry": "myStepEntry",
	"steps": [
		{
			"type": "agent.text",
			"id": "myId",
			"name": "Welcome",
			"entry": "myId-Welcome",
			"message": "Hi this is a message with button options",
			"exit": "myStep.entry",
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
			"message":"Thanks for filling out our step",
			"exit":""
		}
	]
}'

```

**Journeys** are a series of steps stitched together to create a uniquely identifiable user journey.

**Note:** We do not use tree structures but rather the `entry` and `exit` convention to connect steps together.

### HTTPS Request

`POST https://api.chatinc.com/create/journey`

### POST Parameters


Field | Required | Description
--------- | ------- | -----------
journeyId | true | A unique identifier for this journey
entry | true | This the entry value of the step that will be the first to be called in this journey. Entry can also be a keyword. If the entry value does not match to any step, this value will be treated as a key word.
steps |true | An array of step objects.

### Retrieving journeys

> Call this request to retrieve journeys:

```shell
curl --request GET \
  --url https://api.chatinc.com/v1/retrieve/journeys\
  --header 'Authorization: [API Key]' \
```

> Calling this request returns a json object structured as follows:

```json

[
  {
	"journeyId": "Welcome",
	"entry": "welcome",
	"steps": [
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

`GET https://api.chatinc.com/retrieve/journeys`

Even when you haven't created a journey you'll always receive the **Welcome** Journey because it is a system created journey. The **Welcome** is the default journey and is activated when a user says *Hi*.


### Editing journeys

> Use this endpoint to edit a step or steps within a journey.

```shell
curl --request POST \
  --url https://api.chatinc.com/edit/journey \
  --header 'Authorization: [API Key]' \
  --header 'Content-Type: application/json' \
  --data '{
	"journeyId": "Welcome",
	"entry": "welcome",
	"steps": [
		{ 
			"type":"text",
			"id":"myId",
			"name":"Welcome",
			"entry":"myId-welcome",
			"message":"Hey there welcome to your chatbot. Today!",
			"exit":""
		}
	]
}'
```

To edit a journey you mainly want to edit existing steps within the journey or add new ones. To edit existing steps within a journey you want to change certain fields within a step but not the step's `id` or `entry`. So just keep the `id` and `entry` the same and change the fields you wish. To add new steps just add the steps you wish to add within the request.

You can change the `entry` for the journey by specifying new values as well.

### Post Parameters

Field | Required | Description
--------- | ------- | -----------
journeyId | true | A unique identifier for this journey
entry | optional | This the entry value of the step that will be the first to be called in this journey. Entry can also be a keyword. If the entry value does not match to any step, this value will be treated as a key word.
steps |optional | An array of step objects.


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
			"type": "journey",
			"typeId": "myJourneyId",
			"runAt": "",
			"runAfter": ""
		}
	]
}'
```

Tasks are a special way to perform timed activities resulting in prompting a user. For example you might want to schedule when to send a campaign message or you might want to prompt a user who didn't complete a step or you might want to run a series of surveys at different times but resulting as the same campaign. 

### Tags

You may have noticed tags when we were talking about [steps](#steps). Tags are attributes given to a chat when they interact with a specific step. For example a user who clicks a "Remind Me Later** button and goes on to see a *"Alright we will remind you later"* message will have to be tagged so that we know to perform that specific task. 

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
type | yes | **String**. One of three supported types: "template", "journey" or "message"
typeId | yes | **String**. The id of the "template", "journey" or "message" that will be sent as payload for this task.
runAt | optional | The precise datetime of when this task should run.
runAfter | optional | **String**. Value in minutes after inactivity from the user when this task should run.
overridingTag | recommended | It is also almost always the case that a users interaction with your channel will change wether or not a tag added prior should still be present on the chat. As such you can specify a tag which once is added to the user would render a prior tag obsolete. See [recipes](#recipes) to see this in action.





















