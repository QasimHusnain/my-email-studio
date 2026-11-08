---
paths:
  - "campaigns/**/emails/*.md"
---

# Rules for email files

These rules apply to every email file in any campaign. Each email file must follow them all.

## Top of the file

- The first line is `Subject: ...`, and the subject is 50 characters or fewer.
- The second line is `Preview: ...`, and the preview text is 90 characters or fewer.

## Greeting

- Greet the reader with `{{first_name}}`, for example "Hi {{first_name}},".

## The one main link

- Have exactly one main link, written exactly like this:
  [Book your service](https://brightwellheating.example/contact/)
- Put "Call or WhatsApp 07700900123" beside it as plain text. The phone number is not a second link.

## End of the email

Close in this order:

1. Our sign-off from `brand/voice.md`:

   Thanks,
   Daniel at Brightwell Heating

2. The footer links from the "Email layout" section of `brand/voice.md`:
   - Website: https://brightwellheating.example/
   - 5-star rated on Google – read our 75+ reviews: https://maps.example.com/brightwell-heating
   - Check Daniel's Gas Safe registration: https://www.gassaferegister.co.uk/ (No. 000000)

3. This two-line footer, written as plain text and not inside a code block:

   You're getting this because you've used Brightwell Heating before or asked us for a quote.
   Brightwell Heating Ltd, {{company_address}} · Unsubscribe: {{unsubscribe_url}}
