# Getting Started with Bots

To get started with constructing Chat Bots through the API one essentially only needs to understand the following concepts.

* **Bots** are made up of one or many [Journeys](#getting-started-with-journeys)
* Journeys are categorised into four (4) Journey Types 
* Journeys are made up of [Steps](#getting-started-with-steps), 
* Steps are joined together using [Exit](#exit) and [Entry](#entry) points for each
* Some Steps can have multiple Exit points which are used for branching
* Steps make use of three (3) [Step Types](#step-types) to help you build any Journey
* You can link Tasks and Tags to Steps and Journeys
* Tasks can be grouped together
* Lastly, Journeys can be linked to broadcast Campaigns

## Collecting Journey Data 

* Steps records user inputs to InputGroups. Think of fields being filled out on a form
* Journey Analytics are recorded in an AnalyticsGroup

For simplicity we will be using the following use cases in the code samples. 

* Journey Type: Form 		    - Mortgage Loan Application, Registration Process
* Journey Type: Catalog 	  - Adding items to a cart and checking out
* Journey Type: Chat 		    - Break out to a chat with a live agent
* Journey Type: AI Assist 	- Provide self-service assistance or suppor

