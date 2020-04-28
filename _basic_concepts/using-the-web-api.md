---
title: Using the web API
---
You can call any Web API method using the WebClient provided to your Bolt app as app.client (given that your app has the appropriate scopes). When you call one the client’s methods, it returns a Promise containing the response from Slack.

The token used to initialize Bolt can be found in the context object, which is required for most Web API methods.

```js
// Unix Epoch time for September 30, 2019 11:59:59 PM
const whenSeptemberEnds = 1569887999;

app.message('wake me up', async ({ message, context }) => {
  try {
    // Call the chat.scheduleMessage method with a token
    const result = await app.client.chat.scheduleMessage({
      // The token you used to initialize your app is stored in the `context` object
      token: context.botToken,
      channel: message.channel.id,
      post_at: whenSeptemberEnds,
      text: 'Summer has come and passed'
    });
  }
  catch (error) {
    console.error(error);
  }
});
```
