---
description: >-
  Dummy responses are great but we want to fetch our data from a database since
  we can't add everyone at the entity values.
---

# Final Code

1.Click on `fulfilment` on left menu

2.In the `Inline Editor(Powered by Cloud Functions for Firebase)` you can add the final code below

3. Click the `deploy` button.

![Firebase deploy button](.gitbook/assets/screen-shot-2018-07-19-at-08.47.27.png)

{% hint style="info" %}
**IMPORTANT**: **Make sure you have your own Database URL, instead of**: [`https://gdg-cloud-fc257.firebaseio.com/`](https://gdg-cloud-fc257.firebaseio.com/) 
{% endhint %}

{% code-tabs %}
{% code-tabs-item title="index.js" %}
```javascript
const functions = require(firebase-functions); 
const { WebhookClient } = require(dialogflow-fulfillment); 
const { Card, Suggestion } = require(dialogflow-fulfillment);
process.env.DEBUG = 'dialogflow:debug'; 
// enables lib debugging statements
var admin = require(firebase-admin); 
  admin.initializeApp({ databaseURL: `https://gdg-cloud-fc257.firebaseio.com/`
});
var db = admin.database();
process.env.DEBUG = "dialogflow:debug"; // enables lib debugging statements
exports.dialogflowFirebaseFulfillment = functions.https.onRequest( (request, response) => { const agent = new WebhookClient({ request, response }); console.log( "Dialogflow Request headers: " + JSON.stringify(request.headers) ); console.log("Dialogflow Request body: " + JSON.stringify(request.body));
function welcome(agent) {
  agent.add(`Hello! Welcome to the GDG Cloud directory conversational app. Would you like to find a lead by 1. Name,  2. Skill`);
}

function fallback(agent) {
  agent.add(`Sorry but could you say that one more time?`);
  agent.add(`I missed what you said. Say it again?`);
}

function getSearchTerm(agent) {
  if (agent.parameters.search_terms == 1) {
    agent.add(`What's the GDG Cloud lead's name?`);
  } else {
    agent.add(`Which skill are you looking for?`);
  }
}

function searchName(agent) {
  var results = db.ref("myDatabase");
  results
    .orderByChild("Name")
    .startAt(agent.parameters.person_name)
    .endAt(`${agent.parameters.person_name}~`)
    .on("value", function(snapshot) {
      snapshot.forEach(function(data) {
          console.log(`The GDG Cloud lead's email is ${data.val().Email}`);
        agent.add(`The GDG Cloud lead's email is ${data.val().Email}`);
      });
    });
}
​
function searchSkill(agent) {
  var scoresRef = db.ref("myDatabase");
  scoresRef
    .orderByChild("Skill")
    .equalTo(agent.parameters.person_skill)
    .on("value", function(snapshot) {
      snapshot.forEach(function(data) {
        agent.add(
          "The GDG Cloud lead expert at " +
            data.val().Skill +
            " is " +
            data.val().Name
        );
      });
    });
}
​
let intentMap = new Map();
intentMap.set('Default Welcome Intent', welcome);
intentMap.set('Default Fallback Intent', fallback);
intentMap.set("Default Welcome Intent - search", getSearchTerm);
intentMap.set("Default Welcome Intent - search - get_name", searchName);
intentMap.set("Default Welcome Intent - search - get_skill", searchSkill);
agent.handleRequest(intentMap);
} );
```
{% endcode-tabs-item %}

{% code-tabs-item title="package.json" %}
```javascript
{ "name": "dialogflowFirebaseFulfillment", "description": "This is the default fulfillment for a Dialogflow agents using Cloud Functions for Firebase", "version": "0.0.1", "private": true, "license": "Apache Version 2.0", "author": "Google Inc.", "engines": { "node": "~6.0" }, "scripts": { "start": "firebase serve --only functions:dialogflowFirebaseFulfillment", "deploy": "firebase deploy --only functions:dialogflowFirebaseFulfillment" }, "dependencies": { "actions-on-google": "2.0.0-alpha.4", "firebase-admin": "^5.4.2", "firebase-functions": "^0.5.7", "dialogflow": "^0.1.0", "dialogflow-fulfillment": "0.3.0-beta.3" }}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

And the `package.json` tab:

{% code-tabs %}
{% code-tabs-item title="package.json" %}
```javascript
{ "name": "dialogflowFirebaseFulfillment", "description": "This is the default fulfillment for a Dialogflow agents using Cloud Functions for Firebase", "version": "0.0.1", "private": true, "license": "Apache Version 2.0", "author": "Google Inc.", "engines": { "node": "~6.0" }, "scripts": { "start": "firebase serve --only functions:dialogflowFirebaseFulfillment", "deploy": "firebase deploy --only functions:dialogflowFirebaseFulfillment" }, "dependencies": { "actions-on-google": "2.0.0-alpha.4", "firebase-admin": "^5.4.2", "firebase-functions": "^0.5.7", "dialogflow": "^0.1.0", "dialogflow-fulfillment": "0.3.0-beta.3" }}
```
{% endcode-tabs-item %}
{% endcode-tabs %}



