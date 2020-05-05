---
title: Updating and pushing views
lang: en
slug: updating-pushing-views
order: 11
---
<div class="primary-wrapper" markdown="1">
  <div class="section-description" markdown="1">
Modals contain a stack of views. When you call
[`views.open`](https://api.slack.com/methods/views.open), you add the root view
to the modal. After the initial call, you can dynamically update a view by
calling [`views.update`](https://api.slack.com/methods/views.update), or stack a
new view on top of the root view by calling
[`views.push`](https://api.slack.com/methods/views.push).

**`views.update`**<br />
To update a view, you can use the built-in client to call `views.update` with
the `view_id` that was generated when you opened the view, and the updated
`blocks` array. If you're updating the view when a user interacts with an
element inside of an existing view, the `view_id` will be available in the
`body` of the request.

**`views.push`**<br />
To push a new view onto the view stack, you can use the built-in client to call
`views.push` with a valid `trigger_id` a new
[view payload](https://api.slack.com/reference/block-kit/views). The arguments
for `views.push` is the same as [opening modals](#creating-modals). After you
open a modal, you may only push two additional views onto the view stack.

Learn more about updating and pushing views in our
[API documentation](https://api.slack.com/surfaces/modals/using#modifying).
</div>

```ruby
# Listen for a button invocation with action_id `button_abc` (assume it's inside of a modal)
action 'button_abc' do |ack, body, context|
  # Acknowledge the button request
  ack

  const result = client.views.update({
    token: context.botToken,
    # Pass the view_id
    view_id: body.view.id,
    # View payload with updated blocks
    view: {
      type: 'modal',
      # View identifier
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
  })
  Rails.logger.debug(result)
rescue Slack::Client::BaseError => error
  Rails.logger.error(error)
end
```
</div>
