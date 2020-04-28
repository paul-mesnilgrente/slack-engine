---
title: Listening for view submissions
---
If a view payload contains any input blocks, you must listen to view_submission events to receive their values. To listen to view_submission events, you can use the built-in view() method.

`view()` requires a callback_id of type string or RegExp.

You can access the value of the input blocks by accessing the state object. state contains a values object that uses the block_id and unique action_id to store the input values.

Read more about view submissions in our API documentation.

```js
// Handle a view_submission event
app.view('view_b', async ({ ack, body, view, context }) => {
  // Acknowledge the view_submission event
  await ack();

  // Do whatever you want with the input data - here we're saving it to a DB then sending the user a verifcation of their submission

  // Assume there's an input block with `block_1` as the block_id and `input_a`
  const val = view['state']['values']['block_1']['input_a'];
  const user = body['user']['id'];

  // Message to send user
  let msg = '';
  // Save to DB
  const results = await db.set(user.input, val);

  if (results) {
    // DB save was successful
    msg = 'Your submission was successful';
  } else {
    msg = 'There was an error with your submission';
  }

  // Message the user
  try {
    await app.client.chat.postMessage({
      token: context.botToken,
      channel: user,
      text: msg
    });
  }
  catch (error) {
    console.error(error);
  }

});
```
