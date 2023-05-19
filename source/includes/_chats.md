# Contacts

The Contacts API is how you manage your users. With this API you can create and manage [audiences](#audiences), [variables](#variables) and [variable groups](#variable-groups).

## Audiences

Audiences are how you segment your users. You can create audiences based on user attributes, such as location, or based on user behavior, such as whether they have purchased from you before. You can then use these audiences to target your users with specific campaigns and or tasks.

### Create Audience

> Use this endpoint to create an audience

```javascript

chat.createAudience({
  id: 'myAudienceId',
  name: 'My Audience',
  description: 'This is my audience',
  contacts:[{
    number: '1234567890',
  }]
}).then((response) => {
  console.log(response);
}).catch((error) => {
  console.log(error);
});

```

```shell

```

### HTTPS Request

`POST http://api.chatinc.com/v1/create/audience`



### Add Contacts to Audience

> Use this endpoint to add contacts to an audience

```javascript

chat.addContactsToAudience({
  id: 'myAudienceId',
  contacts:[{
    number: '1234567890',
  }]
}).then((response) => {
  console.log(response);
}).catch((error) => {
  console.log(error);
});

```

```shell

```

### HTTPS Request

`POST http://api.chatinc.com/v1/create/audience`

### Retrieve Audiences

> Use this endpoint to retrieve all your audiences

```javascript

chat.retrieveAudiences().then((response) => {
  console.log(response);
}).catch((error) => {
  console.log(error);
});

```

```shell

```

### HTTPS Request

`GET http://api.chatinc.com/v1/retrieve/audiences`

### Retrieve Audience Contacts

> Use this endpoint to retrieve details about a particular audience

```javascript

chat.retrieveAudienceCotacts({
  id: 'myAudienceId'
}).then((response) => {
  console.log(response);
}).catch((error) => {
  console.log(error);
});

```

```shell

```

### HTTPS Request

`POST http://api.chatinc.com/v1/retrieve/audience/contacts`



## Variables

Variables are the data that you can use to personalize your messages. You can use variables to personalize your messages with the user's name, or to personalize your messages with the user's location. You can also use variables to personalize your messages with the user's order history.
## Variable Groups

Variable groups are a group of variables that are related to each other. For example, if you want to send a message to a user that says "Hello, John Doe. How are you today?" you would create a variable group called `user` and add two variables to it, `first_name` and `last_name`. You would then use the variable group in your message like so: `Hello, {user.first_name} {user.last_name}. How are you today?`

### Create Variable Group

> Use this endpoint to create a variable group

```javascript

chat.createVariableGroup({
  id: 'myVariableGroupId',
  name: 'My Variable Group',
  description: 'This is my variable group',
  fields: ["first_name", "last_name"]
  values: [{
    number: '1234567890',
    first_name: "John",
    last_name: "Doe"
  }]
}).then((response) => {
  console.log(response);
}).catch((error) => {
  console.log(error);
});

```

```shell

```

### HTTPS Request

`POST http://api.chatinc.com/v1/create/variable/group`

### Retrieve Variable Groups

> Use this endpoint to retrieve all your variable groups

```javascript

chat.retrieveVariableGroups().then((response) => {
  console.log(response);
}).catch((error) => {
  console.log(error);
});

```

```shell

```

### HTTPS Request

`POST http://api.chatinc.com/v1/retrieve/variable/groups`








