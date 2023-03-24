# Recipes😋 (Advanced)

Recipes combine a wide range of components to provide any number of robust use cases. With a knowledge of:

1. [Campaigns](#campaingns), 
2. [Flows](#flows), 
3. [Tasks](#tasks)
4. [Chats](#chats) 

Here is how you can bring advanced functionality to your channel. Feel free to revisit the sections to refresh your knowledge.⚡💡🔄

## Use Case 1: Campaign - RSVP

> Step 1: Create "Maybe Next Time" Flow:

```shell

curl --request POST \
  --url https://api.chatinc.com/create/flow \
  --header 'Authorization: [API Key}' \
  --header 'Content-Type: application/json' \
  --data '{
	"flowId": "maybeLaterFlow",
	"entry": "thankYouMessage",
	"forms": [
		{
			"type": "agent.text",
			"id": "thankYouMessage",
			"name": "Thank You",
			"entry": "thankYouMessage",
			"message": "Thanks for your response. We understand this event might not be for you. We looked forward to sending you other relevant events",
			"exit": "",
			
}'

```

> Step 2: Create "I'm Attending" Flow:

```shell
  curl --request POST \
  --url https://api.chatinc.com/create/flow \
  --header 'Authorization: [API Key}' \
  --header 'Content-Type: application/json' \
  --data '{
	"flowId": "imAttendingFlow",
	"entry": "imAttendingSurvey",
	"forms": [
		{
			"type": "agent.survey",
			"id": "imAttendingSurvey",
			"name": "RSVP Survey",
			"entry": "imAttendingSurvey",
			"message": "Please answer the below questions",
			"exit": "surveyCompleteMessage",
			"questions": [
				{
					"id": 1,
					"name": "Dietary requirements",
					"options": ["None", "Halaal", "Vegetarian"],
					"message": " What are your dietary requirements with a drop down list of all the options.",
					"validate": ""
				},
				{
					"id": 2,
					"name": "People Attending",
					"options": ["1", "2", "3"],
					"message": "How many people will be attending?",
					
				},
        {
					"id": 3,
					"name": "Special requirements",
					"options": ["None"],
					"message": "Please enter any other special requirements by typing your request or simply click the None button to complete the RSVP process.",
				},
         {
					"id": 4,
					"name": "Future Messaging",
					"options": ["Yes", "No"],
					"message": "Can we send you communication prior to and after the event? ",
				}
			]
		},
		{ 
			"type":"agent.text",
			"id":"surveyCompleteMessage",
           "tag":["RSVP"]
			"name":"Thank You",
			"entry":"surveyCompleteMessage",
			"message":"Thanks for the RSVP. Please arrive at CTCC at 09:00 on Thur 22, May, 2022",
			"exit":""
		}
	]
}'
```

> Step 3: Create "RSVP" Campaign:

```shell

curl --request POST \
  --url https://api.chatinc.com/v1/create/campaign \
  --header 'Authorization: [API Key]' \
  --header 'Content-Type: application/json' \
  --data '{
	"title": "Super RSVP Campaign",
	"langauageCode": "enUS",
	"templateType": "MARKETING",
	"message": "Hi! We have a special event we want you attend. Please select a option from below:",
	"buttonType": "QUICK_REPLY",
	"buttons": [
		{
			"text": "Im attending",
			"flowId": "imAttendingFlow"
		},
    {
			"text": "Maybe next time",
			"flowId": "maybeLaterFlow"
		}
	],
"sendImmediatley":true
}'

```

> Step 4: Create RSVP Reminder Flow

```shell

curl --request POST \
  --url https://api.chatinc.com/create/flow \
  --header 'Authorization: [API Key}' \
  --header 'Content-Type: application/json' \
  --data '{
	"flowId": "rsvpRemiderFlow",
	"entry": "reminderMessage",
	"forms": [
		{
			"type": "agent.text",
			"id": "reminderMessage",
			"name": "RSVP Reminder",
			"entry": "reminderMessage",
			"message": "This is a reminder that Super Event is in 2 days!",
			"exit": "",
			
}'

```

> Step 5: Create Reminder Task:

```shell

curl --request POST \
  --url https://api.chatinc.com/create/task/group \
  --header 'Authorization: [API Key]' \
  --header 'Content-Type: application/json' \
  --data '{
	"taskId": "rsvpTask",
	"group": [
		{
			"tag": "RSVP",
			"type": "flow",
			"messageId": "rsvpRemiderFlow",
			"runAt": "3-25-2023.T19:00.00",
		}
	]
}'

```

> Step 6: Retrieve Campaign Results:

```shell

curl --request GET \
  --url https://api.chatinc.com/v1/retrieve/campaign/?campaignId=Super RSVP Campaign\
  --header 'Authorization: [API Key]' \

```


College or business has an event that they would like to invite customers or people to. They prepare a list of contacts that need to receive the invite via WhatsApp (or any other channel in future). They plan to invite 1000 people and need to ask them to RSVP and to tell them their food preferences for the event. They create the personalised invite using a WhatsApp template message, select the contact list containing 1000 people and proceed to create the rest of the campaign. 
From the customer’s perspective they receive a broadcast message from company A inviting them to attend. The message contains everything the person needs to know about the event which is happening in two weeks. 

**Person A** clicks the **“Maybe Next time”** button and receives a follow up thank you message.

**Person B** clicks the button, **“I’m Attending"**, and receives the following list of short questions.

Q1 - What are your dietary requirements with a drop down list of all the options. They choose None and move to the next question.

Q2 - How many people will be attending with a drop down list of all the options. They choose 2 and move to the next question.

Q3 - Please enter any other special requirements by typing your request or simply click the None button to complete the RSVP process.

Q4 - Can we send you communication prior to and after the event?  Yes No options buttons.

**Person B** receives a thank you follow up message with all the required event details.
2 days before the event a reminder is sent out reminding those who have RSVP’d.
The company navigates to the reporting screen to see the results come in over the next few days. They are able to clearly see every metric from

*Total sent*

*Total delivered*

*Total Failures*

*Total Read*

*Total Successful RSVP'd.*

For each question that was asked for the RSVP there is an aggregated number for dietary requirements summarising the results.
Each RSVP can be expanded to check the individual answers.

*Total “Maybe Next Time”*

They are able to download or export the results.




