---
title: Listening and responding to options
lang: en
slug: options
order: 13
---
<div class="primary-wrapper" markdown="1">
  <div class="section-description" markdown="1">
The `option()` method listens for incoming option request payloads from Slack.
[Similar to `actions()`](#action-listening), an `action_id` or constraints
object is required.

While it's recommended to use `action_id` for `external_select` menus, dialogs
do not yet support Block Kit so you'll have to use the constraints object to
filter on a `callback_id`.

To respond to options requests, you'll need to `ack()` with valid options. Both
[external select response examples][external-select] and
[dialog response examples][dynamic-select] can be found on our API site.
  </div>

```ruby
# Example of responding to an external_select options request
options 'external_action' do |options|
  # Get information specific to a team or channel
  results = db.get(options.team.id)

  if results
    # Collect information in options array to send in Slack ack response
    options = results.map do |result|
      {
        "text": {
          "type": "plain_text",
          "text": result.label
        },
        "value": result.value
      }
    end

    ack(options: options)
  else
    ack
  end
end
```
</div>

[external-select]: https://api.slack.com/reference/messaging/block-elements#external-select
[dynamic-select]: https://api.slack.com/dialogs#dynamic_select_elements_external
