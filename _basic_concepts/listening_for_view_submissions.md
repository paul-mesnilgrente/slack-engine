---
title: Listening for view submissions
lang: en
slug: view_submissions
order: 12
---
<div class="primary-wrapper" markdown="1">
  <div class="section-description" markdown="1">
If a [view payload](https://api.slack.com/reference/block-kit/views)
contains any input blocks, you must listen to `view_submission`
events to receive their values. To listen to `view_submission`
events, you can use the built-in `view()` method.

`view()` requires a `callback_id` of type `string` or `RegExp`.

You can access the value of the `input` blocks by accessing the `state` object.
`state` contains a `values` object that uses the `block_id` and unique
`action_id` to store the input values.

Read more about view submissions in our [API documentation][using-interactions].
</div>

```ruby
# Handle a view_submission event
view('view_b' do |body, view, context|
  # Acknowledge the view_submission event
   ack

  # Do whatever you want with the input data - here we're saving it to a DB then
  # sending the user a verifcation of their submission

  # Assume there's an input block with `block_1` as the block_id and `input_a`
  val = view['state']['values']['block_1']['input_a']
  user = body['user']['id']

  # Save to DB
  results = db.set(user.input, val)

  if results
    # DB save was successful
    msg = 'Your submission was successful'
  else
    msg = 'There was an error with your submission'
  end

  # Message the user
  client.chat.postMessage({
    token: context.botToken,
    channel: user,
    text: msg
  })
rescue Slack::Client::BaseError => error
  Rails.logger.error(error)
end
```

[using-interactions]: https://api.slack.com/surfaces/modals/using#interactions
