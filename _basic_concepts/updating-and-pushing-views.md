---
title: Updating a pushing views messages
---
Updating and pushing views

Modals contain a stack of views. When you call views.open, you add the root view to the modal. After the initial call, you can dynamically update a view by calling views.update, or stack a new view on top of the root view by calling views.push.

`views.update`
To update a view, you can use the built-in client to call views.update with the view_id that was generated when you opened the view, and the updated blocks array. If you’re updating the view when a user interacts with an element inside of an existing view, the view_id will be available in the body of the request.

`views.push`
To push a new view onto the view stack, you can use the built-in client to call views.push with a valid trigger_id a new view payload. The arguments for views.push is the same as opening modals. After you open a modal, you may only push two additional views onto the view stack.

Learn more about updating and pushing views in our API documentation.

```js
// Listen for a button invocation with action_id `button_abc` (assume it's inside of a modal)
app.action('button_abc', async ({ ack, body, context }) => {
  // Acknowledge the button request
  await ack();

  try {
    const result = await app.client.views.update({
      token: context.botToken,
      // Pass the view_id
      view_id: body.view.id,
      // View payload with updated blocks
      view: {
        type: 'modal',
        // View identifier
        callback_id: 'view_1',
        title: {
          type: 'plain_text',
          text: 'Updated modal'
        },
        blocks: [
          {
            type: 'section',
            text: {
              type: 'plain_text',
              text: 'You updated the modal!'
            }
          },
          {
            type: 'image',
            image_url: 'https://media.giphy.com/media/SVZGEcYt7brkFUyU90/giphy.gif',
            alt_text: 'Yay! The modal was updated'
          }
        ]
      }
    });
    console.log(result);
  }
  catch (error) {
    console.error(error);
  }
});
```
