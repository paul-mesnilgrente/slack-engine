---
title: Handling errors
lang: en
slug: error-handling
order: 1
---
<div class="primary-wrapper" markdown="1">
  <div class="section-description" markdown="1">
If an error occurs in a listener, it’s recommended you handle it directly with
a `try`/`catch`.
However, there still may be cases where errors slip through the cracks. By
default, these errors will be logged to the console. To handle them yourself,
you can attach a global error handler to your app with the `app.error(fn)`
method.
</div>

```ruby
class SlackController < Slack::BaseController
  rescue_from Slack::BaseError with: handle_errors

private
  # Check the details of the error to handle cases where you should retry sending a message or stop the app
  def handle_errors(error)
    Rails.logger.error(error)
  end
end
```
