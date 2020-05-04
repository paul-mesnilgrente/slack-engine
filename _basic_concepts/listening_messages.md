---
title: Listening to messages
lang: en
slug: message-listening
order: 1
---

<div class="primary-wrapper" markdown="1">
  <div class="section-description" markdown="1">
To listen to messages that [your app has access to receive](https://api.slack.com/messaging/retrieving#permissions), you can use the `message()` method which filters out events that aren’t of type `message`.

`message()` accepts an optional `pattern` parameter of type `string` or `RegExp` object which filters out any messages that don’t match the pattern.
  </div>

```javascript
// This will match any message that contains 👋
app.message(':wave:', async ({ message, say }) => {
  await say(`Hello, <@${message.user}>`);
});
```
</div>


<details class="secondary-wrapper">
  <summary class="section-head">
    <h4>Using a RegExp pattern</h4>
  </summary>

  <div class="details-content" markdown="1">

<div class="section-description" markdown="1">
A RegExp pattern can be used instead of a string for more granular matching.

All of the results of the RegExp match will be in `context.matches`.
</div>

```javascript
app.message(/^(hi|hello|hey).*/, async ({ context, say }) => {
  // RegExp matches are inside of context.matches
  const greeting = context.matches[0];

  await say(`${greeting}, how are you?`);
});
```

</div>
</details>
