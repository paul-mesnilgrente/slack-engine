---
title: Listening to messages
---
To listen to messages that your app has access to receive, you can use the
message() method which filters out events that aren’t of type message.

message() accepts an optional pattern parameter of type string or RegExp object
which filters out any messages that don’t match the pattern.

```javascript
// This will match any message that contains 👋
app.message(':wave:', async ({ message, say }) => {
  await say(`Hello, <@${message.user}>`);
});
```

**Using a RegExp pattern**

A RegExp pattern can be used instead of a string for more granular matching.

All of the results of the RegExp match will be in`context.matches`.

```javascript
app.message(/^(hi|hello|hey).*/, async ({ context, say }) => {
  // RegExp matches are inside of context.matches
  const greeting = context.matches[0];

  await say(`${greeting}, how are you?`);
});
```
