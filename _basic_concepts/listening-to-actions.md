---
title: Listening to actions
---
Your app can listen to user actions like button clicks, and menu selects, using the action method.

Actions can be filtered on an action_id of type string or RegExp object. action_ids act as unique identifiers for interactive components on the Slack platform.

You’ll notice in all action() examples, ack() is used. It is required to call the ack() function within an action listener to acknowledge that the event was received from Slack. This is discussed in the acknowledging events section.

Note: Since v2, message shortcuts (previously message actions) now use the shortcut() method instead of the action() method. View the migration guide for V2 to learn about the changes.

```js
// Your middleware will be called every time an interactive component with the action_id "approve_button" is triggered
app.action('approve_button', async ({ ack, say }) => {
  await ack();
  // Update the message to reflect the action
});
```

Listening to actions using a constraint object

You can use a constraints object to listen to callback_ids, block_ids, and action_ids (or any combination of them). Constraints in the object can be of type string or RegExp object.

```js
// Your middleware will only be called when the action_id matches 'select_user' AND the block_id matches 'assign_ticket'
app.action({ action_id: 'select_user', block_id: 'assign_ticket' },
  async ({ action, ack, context }) => {
    await ack();
    try {
      const result = await app.client.reactions.add({
        token: context.botToken,
        name: 'white_check_mark',
        timestamp: action.ts,
        channel: action.channel.id
      });
    }
    catch (error) {
      console.error(error);
    }
  });
```
