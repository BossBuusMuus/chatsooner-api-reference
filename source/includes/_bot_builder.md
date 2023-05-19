# Bot Builder
## Getting Started with Journeys

> Use this request to create a journey

```javascript

chat.createJourney({
  journeyId: 'myJourneyId',
  entry: 'myStepEntry',
  steps: [
    {
      type: 'inputRequest',
      name: 'Welcome',
      entry: 'myStepEntry',
      message: 'Please enter your name',
      exit: 'myId-Thank You',
      score: 1,
      validate: {
        type: 'number',
        min: 1
      },
      skip: false
    },
    {
      type: 'agent.text',
      id: 'myId',
      name: 'Thank You',
      entry: 'myId-Thank You',
      message: 'Thanks for filling out our step',
      exit: ''
    }
  ]
}).then((response) => {
  console.log(response);
}).catch((error) => {
  console.log(error);
})

```

```shell
curl --request POST \
  --url https://api.chatinc.com/v1/create/journey \
  --header 'Authorization: [API Key}' \
  --header 'Content-Type: application/json' \
  --data '{
	"journeyId": "myJourneyId",
	"entry": "myStepEntry",
	"steps": [
		{ 
  
  "type":"inputRequest",
  "name":"Welcome",
  "entry":"myStepEntry",
  "message":"Please enter your name",
  "exit":"myId-Thank You",
  "score":1,
  "validate":{
    "type":"number",
    "min":1
  },
  "skip":false
}
		{ 
			"type":"text",
			"name":"Thank You",
			"entry":"myId-Thank You",
			"message":"Thanks for filling out our step",
			"exit":""
		}
	]
}'

```

**JOURNEYS** are synonymous with the term flows, as it is often termed in other platforms, however we will stay with the term journey. Journeys usually have a specific function or use case they need to perform, such as completing a form, or “filling out” a loan application or completing a registration or sign up process. 

In the example of a signup process, we can see that a signup process comprises collecting information (InputRequest) to complete the signup through a series of Steps in the form of questions asked of the user. We refer to these requests for input as STEPS. Each step has a specific function to perform in the Journey and different Steps can have a variety of different functions they perform. These various functions are referred to as STEP TYPES and because the types of Journeys (use cases) can differ markedly we also have the concept called JOURNEY TYPES where specific Step Types relate to specific to certain Journey Types for example Journey Type.

Form has the following Step Types available to it during the Bot build or design process:

* Step Type: [InputRequest](#inputrequest)
* Step Type: [InputOptions](#inputoptions)
* Step Type: [Text](#text)

A step can be a choice the user needs to make, an information capture request, a confirmation step, a lookup request. So to conclude, Bots can have either a single journey or multiple journeys and each journey has a specific function, and consists of steps and you use Step Types to build out the journey.

Lastly each STEP has an Entry point and an Exit point and you join up a sequence of Steps by specifying an Entry and Exit point for each. Occasionally you may need a Step that branches out into multiple options and in such a case you will use the Step Type that allows for multiple Exit points.




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

```javascript

chat.retrieveJourneys().then((response) => {
  console.log(response);
}).catch((error) => {
  console.log(error);
})

```

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
			"type":"text",
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


```javascript

chat.editJourney({
  journeyId: 'Welcome',
  entry: 'welcome',
  steps: [
    {
      type: 'text',
      id: 'myId',
      name: 'Welcome',
      entry: 'myId-welcome',
      message: 'Hey there welcome to your chatbot. Today!',
      exit: ''
    }
  ]
}).then((response) => {
  console.log(response);
}).catch((error) => {
  console.log(error);
})

```

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


## Getting Started with Steps

Below are some essential concepts to understand when working with steps.

### Collecting Inputs 

Inputs can be grouped together and stored through the use of a data group. Data Groups are special data objects that store input from steps that have a dataGroupId specified. See more about creating Data Groups here.

### Collecting Analytics 

It is often beneficial to be able to view how your users move around in your bot journeys and for this we have special data objects known as Analytics Groups. An Analytics Group is able to capture every single step a user goes through. To be able to capture a step and add it to an analytics group you specify the analyticsGroupId on the step.
See more about creating analytics groups here.

### Entry

(unique identifier for a step. Meant to be a human readable identifier for debugging purposes) an entry is how a step is identified within a journey. It is also referenced in the exit as means to identify the next step. See exit next

### Exit
An exit points out to the next step/or journey after a user interacts with the current step. The exit value can be specified as string or a key-value mapping. The keys would be options and the values would be entry values that identify the step that corresponds to each option.


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

## Step Types

You would want to display different types of steps and for that we have various step types.
###  InputRequest

InputRequests serve are unstructured requests for input from a user using conversational messages. You create a form by joining steps together. InputRequest belongs to the Form Journey Type.

* Supports free text input only or uploading documents. 
* No support for interactive elements such as buttons or lists. 
* Supports lead scoring to weight users interactions 
* Step Types provides the ability to do journey scoring to use during lead capture scenarios or other use cases where you may wish  to score or weight the individual users interactions as a means to filter in the output manager.
* Supports validation
* Supports the collection of Journey Analytics

> The object for the step type InputRequest looks as follows:

```json

{ 
  
  "type":"inputRequest",
  "name":"Welcome",
  "entry":"Welcome",
  "message":"Please enter your name",
  "exit":"myId2-OkayStep",
  "dataGroupId":"myDataGroupId",
  "analyticsGroupId":"myAnalyticsGroupId",
  "score":1,
  "validate":{
    "type":"number",
    "min":1
  },
  "skip":false
}
```

Field | Required | Description
--------- | ------- | -----------
entry | true | **string**. A unique identifier for this step. The entry value is used to identify the step within a journey. It is also referenced in the exit as means to identify the next step. See exit next.
exit | true | **string**. An exit points out to the next step/or journey after a user interacts with the current step. The exit is a key-value mapping. The keys would be options and the values would be entry values that identify the step that corresponds to each option.
message | optional | **string**. The message to be displayed to the user.
dataGroupId | optional | **string**. The id of the data group to which the input will be saved. This is used to save the input to a group of inputs that can be used to create a data object that can be used to create a lead or other use cases.
analyticsGroupId | optional | **string**. The id of the analytics group to which the input will be saved. This is used to save the input to a group of inputs that can be used to create a data object that can be used to create a lead or other use cases.
score | optional | **number**. The score to be assigned to the user's interaction with this step. This is used to score the user's interaction with the bot and can be used to filter the output manager.
validate | optional | **object**. The validate object is used to validate the input. The validate object has the following fields.
validate.type | optional | **string**. The type of validation to be performed. The available types are **number** and **string**.
validate.min | optional | **number**. The minimum value for the input. This is only used if the type is **number**.
skip | optional | **boolean**. If set to true. adds a skip button to the step.


### InputOptions

InputOptions are also requests for input but contain interactive elements such as buttons or lists that lets the user make a selection or choice by using buttons or lists i.e a  structured input. InputOptions can have multiple Exit points to allow for branching at certain steps. Use InputOptions to provide users with the ability to make a quick selection for example in the case of a mortgage loan application the user might be presented with a range of mortgage types to select from, and based on the mortgage type selected will be presented with the relevant steps to be completed for that particular mortgage type application. These selection buttons or lists can also be programmed to access other parts of the same journey or other journeys within the bot or even hidden journeys that  are only accessible via certain triggers. The InputOptions Step Type provides the ability to do journey scoring to use during lead capture scenarios or other use cases where you may wish  to score or weight the individual users interactions as a means to filter in the output manager. The following options are available for the InputOptions Step Type.

> The object for the step type InputOptions looks as follows:

```json

{ 
  
  "type":"inputOptions",
  "name":"Welcome",
  "entry":"Welcome",
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
  "dataGroupId":"myDataGroupId",
  "analyticsGroupId":"myAnalyticsGroupId",
  "score":1
}
```

Field | Required | Description
--------- | ------- | -----------
options | true | A array of the values that will be shown to the user as buttons or in a list. You should specify these values as objects with **title** and **description** fields to allow for menu lists with descriptions.
header | optional | The header of the message
innerTitle | optional | The header of the menu list.
entry | true | **string**. A unique identifier for this step. The entry value is used to identify the step within a journey. It is also referenced in the exit as means to identify the next step. See exit next.
exit | true | **object**. An exit points out to the next step/or journey after a user interacts with the current step. The exit is a key-value mapping. The keys would be options and the values would be entry values that identify the step that corresponds to each option.
message | optional | **string**. The message to be displayed to the user.
dataGroupId | optional | **string**. The id of the data group to which the input will be saved. This is used to save the input to a group of inputs that can be used to create a data object that can be used to create a lead or other use cases.
analyticsGroupId | optional | **string**. The id of the analytics group to which the input will be saved. This is used to save the input to a group of inputs that can be used to create a data object that can be used to create a lead or other use cases.
score | optional | **number**. The score to be assigned to the user's interaction with this step. This is used to score the user's interaction with the bot and can be used to filter the output manager.
validate | optional | **object**. The validate object is used to validate the input. The validate object has the following fields.
validate.type | optional | **string**. The type of validation to be performed. The available types are **number** and **string**.
validate.min | optional | **number**. The minimum value for the input. This is only used if the type is **number**.
skip | optional | **boolean**. If set to true, the step will be skipped if the user's input is invalid. If set to false, the user will be prompted to re-enter the input. The default value is false.


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

Selection can be seen as a form of navigation presented during a journey and presents the user with a number of selection options that allows for branching into various other journeys via multiple exit points. The difference between InputOptions and Selection Step Types  are that Selection does not save the Input to an InputGroup such as with InputOptions which would be required if you wish to collect and view the combined inputs of all the steps as part of an application or registration process for example. 

Field | Required | Description
--------- | ------- | -----------
options | true | A array of the values that will be shown to the user as buttons or in a list. You should specify these values as objects with **title** and **description** fields to allow for menu lists with descriptions.
header | optional | The header of the message
innerTitle | optional | The header of the menu list.
entry | true | **string**. A unique identifier for this step. The entry value is used to identify the step within a journey. It is also referenced in the exit as means to identify the next step. See exit next.
exit | true | **object**. An exit points out to the next step/or journey after a user interacts with the current step. The exit is a key-value mapping. The keys would be options and the values would be entry values that identify the step that corresponds to each option.
message | optional | **string**. The message to be displayed to the user.


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

## Input Groups

Input Groups are used to group inputs together. This is useful for example if you want to collect a user's name and surname as part of a registration process. You can then use the input group to create a data object that can be used to create a lead or other use cases. After creating an input group you can use the input group id to create a journey step of type inputOptions or inputText. The input group id is used to save the input to the input group.

When a user interacts with a step of type inputOptions or inputText, the input is saved to the input group.

### Create Input Group

> Use this endpoint to create an input group

```javascript

chat.createInputGroup({
  id: 'myInputGroup',
  name: 'My Input Group',
  description: 'This is my input group'
}).then((response) => {
  console.log(response);
}).catch((error) => {
  console.log(error);
});

```

```shell

curl -- request POST \
  url https://api.chatinc.com/v1/create/input/group \
  --header 'Authorization: [API Key]' \
  --header 'Content-Type: application/json' \
  --data '{ 
  "id": "myInputGroup",
  "name": "My Input Group",
  "description": "This is my input group"
}'
```

### Get Input Group

> Use this endpoint to get an input group

```javascript
 chat.retrieveInputGroup({
  id: 'myInputGroup'
}).then((response) => {
  console.log(response);
}).catch((error) => {
  console.log(error);
});

```


When you retrieve an input group you will receive all input that was collected from each user interaction with a step of type inputOptions or inputText, grouped together by the input group id and tied to the user's id (phone number).


## Analytics Group

Analytics Groups are used to track and record us




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





















