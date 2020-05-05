---
title: Responding to actions
lang: en
slug: action-respond
order: 6
---
<div class="primary-wrapper" markdown="1">
  <div class="section-description" markdown="1">
There are two main ways to respond to actions. The first (and most common) way
is to use the `say` function. The `say` function sends a message back to the
conversation where the incoming event took place.

The second way to respond to actions is using `respond()`, which is a simple
utility to use the `response_url` associated with an action.
</div>

```ruby
# Your middleware will be called every time an interactive component with the
# action_id “approve_button” is triggered
action 'approve_button' do
  # Acknowledge action request
  ack
  say 'Request approved 👍'
end
```

<details class="secondary-wrapper" markdown="1">
  <summary class="section-head" markdown="0">
    <h4>Using respond()</h4>
</summary>

<div class="details-content" markdown="1">

<div class="section-description" markdown="1">
Since `respond()` is a utility for calling the `response_url`, it behaves in the same way. You can pass a JSON object with a new message payload that will be published back to the source of the original interaction with optional properties like `response_type` (which has a value of `in_channel` or `ephemeral`), `replace_original`, and `delete_original`.
</div>

```ruby
# Listens to actions triggered with action_id of “user_select”
action 'user_choice' do |action|
  ack
  "You selected #{action.selected_user}"
end
```
</div>
</details>
