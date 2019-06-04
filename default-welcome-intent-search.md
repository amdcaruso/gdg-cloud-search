---
description: >-
  Let's create a search intent for getting the search term our users want to
  start their search.
---

# Default Welcome Intent - search

## Steps

1. Click `Intents` from the left menu.
2. On "Default Welcome Intent" hover your mouse to the right on top of it
3. Click on `Add follow-up intent`
4. Choose `Custom`
5. This will take you to the new created follow-up intent
6. Rename from `Default Welcome Intent - custom` to `Default Welcome Intent - search`
7. Click `save`.
8. Click on `fulfilment` on the left menu
9. Go to the firebase inline editor and update your code to look like this:

{% code-tabs %}
{% code-tabs-item title="index.js" %}
```javascript
// See https://github.com/dialogflow/dialogflow-fulfillment-nodejs
// for Dialogflow fulfillment library docs, samples, and to report issues
"use strict";

const functions = require("firebase-functions");
const { WebhookClient } = require("dialogflow-fulfillment");
const { Card, Suggestion } = require("dialogflow-fulfillment");

process.env.DEBUG = "dialogflow:debug"; // enables lib debugging statements

exports.dialogflowFirebaseFulfillment = functions.https.onRequest(
  (request, response) => {
    const agent = new WebhookClient({ request, response });
    console.log(
      "Dialogflow Request headers: " + JSON.stringify(request.headers)
    );
    console.log("Dialogflow Request body: " + JSON.stringify(request.body));

    function welcome(agent) {
      agent.add(
        `Hello! Welcome to the GDG Cloud directory conversational app. Would you like to find a lead by 1. Name,  2. Skill`
      );
    }

    function fallback(agent) {
      agent.add(`I didn't understand`);
      agent.add(`I'm sorry, can you try again?`);
    }
    
    function getSearchTerm(agent) {
      if (agent.parameters.search_terms == 1) {
        agent.add(`What's the GDG Cloud lead's name?`);
      } else {
        agent.add(`Which skill are you looking for?`);
      }
    }

    // Run the proper function handler based on the matched Dialogflow intent name
    let intentMap = new Map();
    intentMap.set("Default Welcome Intent", welcome);
    intentMap.set("Default Fallback Intent", fallback);
    intentMap.set("Default Welcome Intent - search", getSearchTerm);
    agent.handleRequest(intentMap);
  }
);
```
{% endcode-tabs-item %}

{% code-tabs-item title="package.json" %}
```javascript
{
  "name": "dialogflowFirebaseFulfillment",
  "description": "This is the default fulfillment for a Dialogflow agents using Cloud Functions for Firebase",
  "version": "0.0.1",
  "private": true,
  "license": "Apache Version 2.0",
  "author": "Google Inc.",
  "engines": {
    "node": "~6.0"
  },
  "scripts": {
    "start": "firebase serve --only functions:dialogflowFirebaseFulfillment",
    "deploy": "firebase deploy --only functions:dialogflowFirebaseFulfillment"
  },
  "dependencies": {
    "actions-on-google": "2.0.0-alpha.4",
    "firebase-admin": "^4.2.1",
    "firebase-functions": "^0.5.7",
    "dialogflow": "^0.1.0",
    "dialogflow-fulfillment": "0.3.0-beta.3"
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## **Explanation:**

Since we asked our users how they want to search for a person, we need to follow up their answer. To do so we're going to create a follow up intent for the 'Default Welcome Intent'

To create a follow up intent just go to the intents home page and hover your mouse on the intent you want to add a follow up.

At the options given we are going to select the 'custom intent' and rename our custom intent to 'Default Welcome Intent - search'

## Follow-up intent Explanation <a id="explanation"></a>

What if you need to structure workflows to your conversational apps?

But how do we do `if`, `else`, `switch cases`?

What if in case the answer is yes`do something`

otherwise, `do something else.`

Follow-up intents allow you to to shape dialog without having to manage contexts manually. We can make the intents extend each other, as they are part of the same logical group.

But how do we do `if`, `else`, `switch cases`?

What if in case the answer is yes`do something`

otherwise, `do something else.`

Follow-up intents allow you to to shape dialog without having to manage contexts manually. We can make the intents extend each other, as they are part of the same logical group.

