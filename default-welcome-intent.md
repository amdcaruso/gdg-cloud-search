---
description: >-
  The welcome intent is the first message, usually a greeting one. The welcome
  intent is also responsible for informing your users about their options and
  what they could do with your agent.
---

# Default Welcome Intent

## Steps:

1. Click `Intents` from the left menu.
2. Click `Default Welcome Intent`
3. Add some training phrases, add each word or expression, then hit return to add a following one.

4. Add a Response`Hi! Welcome to the GDG Cloud leads directory conversational app. Would you like to find a lead by 1. Name, 2. Skill`

5. Click `save`.

6. On the menu left click on `Fulfilment`

7. Click `enable Inline Editor(Powered by Cloud Functions for Firebase)`

8. Your final code should like like this for now, after we override the welcome function with our change:

```javascript
function welcome(agent) {
  agent.add(`Hello! Welcome to the GDG Cloud directory conversational app. Would you like to find a lead by 1. Name,  2. Skill`);
}
```

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

    // Run the proper function handler based on the matched Dialogflow intent name
    let intentMap = new Map();
    intentMap.set("Default Welcome Intent", welcome);
    intentMap.set("Default Fallback Intent", fallback);
    agent.handleRequest(intentMap);
  }
);

```
{% endcode-tabs-item %}
{% endcode-tabs %}

  


