---
title: Chat Inc API Reference

language_tabs:
  - javascript
  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://chatinc.com'>Get Support</a>

includes:
  - getting_started_with_bots
  - bot_builder
  - campaigns
  - chats
  - ai_assistant
  - catalog
  - recipes

search: true

code_clipboard: true
---

# Welcome (v0.8.5-development)

```javascript

npm install chatinc

```

Welcome to the **Chat Inc API Reference Documentation**. Easily build and launch conversational experiences from the ground up or embed them into your existing infrastructure with fast, secure and scalable APIs😉. 

Feel free to look around and see what you can achieve. This is a living document🛠, likely to be updated everyday😄. 

To get your own API keys go to [chatinc](https://chatinc.com) and create an account. A sandbox is a also available.

> To authorize, use this code:

```javascript
import Chat from 'chatinc';

const API_KEY = 'API_KEY';
const chat = new Chat(API_KEY);

```

```shell
# With shell, you can just pass the correct header with each request
curl "https://api.chatinc.com/end_point"
  -H "Authorization: [Your API Key]"
```

To get started even faster we have a [JS Library](https://www.npmjs.com/package/chatinc) available on npm.


