---
title: Acknowledging events
lang: en
slug: acknowledge
order: 7
---
<div class="primary-wrapper" markdown="1">
  <div class="section-description" markdown="1">
Actions, commands, and options events must **always** be acknowledged using the
`ack()` function. This lets Slack know that the event was received and updates
the Slack user interface accordingly. Depending on the type of event, your
acknowledgement may be different. For example, when acknowledging a dialog
submission you will call `ack()` with validation errors if the submission
contains errors, or with no parameters if the submission is valid.

We recommend calling `ack()` right away before sending a new message or fetching
information from your database since you only have 3 seconds to respond.
  </div>

```ruby
# Regex to determine if this is a valid email
EMAIL_REGEX = /^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$/
# This uses a constraint object to listen for dialog submissions with a callback_id of ticket_submit
action callback_id: 'ticket_submit' do |action|
  # it’s a valid email, accept the submission
  if action.submission.email =~ EMAIL_REGEX
    ack
  else
    # if it isn’t a valid email, acknowledge with an error
    ack(
      errors: [
        name: "email_address",
        error: "Sorry, this isn’t a valid email"
      ]
    )
  end
end
```
</div>
