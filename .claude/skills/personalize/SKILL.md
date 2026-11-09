---
name: personalize
description: Write a version of each campaign email for every lead group in leads/leads.csv. Use when a campaign's emails are final and you want one copy per group.
argument-hint: "[campaign folder]"
context: fork
allowed-tools: Read, Glob, Grep, Write
---

# Personalize a campaign for each lead group

Campaign folder: $ARGUMENTS

## 1. Check the campaign folder

- If no campaign folder was given above, stop. Use Glob on `campaigns/*/` to find the campaign folders that exist, list them, and ask which one to use. Write nothing.
- If the folder doesn't exist or has no `emails/*.md` files, stop and say so, and list the campaign folders that do exist. Write nothing.

## 2. Read what you need

1. `brand/voice.md`: voice, words to avoid, claims we must never make, and the approved reviews.
2. `.claude/rules/emails.md`: the rules every email file must follow.
3. `leads/leads.csv`: read the group names from the `group` column. Use every distinct value found there. Don't assume the names, and don't add groups that aren't in the file. Read only the `group` column; don't copy names or addresses into any email.
4. Every email in `<campaign folder>/emails/*.md`. Only read the top-level `emails/` folder, not any existing `groups/` copies.

## 3. Write one version per email per group

For each group, and each email, keep these exactly as they are in the original:

- The structure and order of the email.
- The `Subject:` line.
- The `{{first_name}}` greeting and every other `{{...}}` placeholder.
- The `[Book your service](https://brightwellheating.example/contact/)` link and the "Call or WhatsApp 07700900123" line beside it.
- The customer review, word for word, with its first name. Never reword, shorten or combine it, and never swap it for a different one.
- The sign-off, the footer links and the two-line footer.

Change only two things:

1. **The opening line** after the greeting.
2. **One example** in the body, so it fits the group.

Fit each group like this:

- **families**: a busy household with young children, where a cold house or no hot water hits hardest.
- **landlords**: keeping tenants warm and having the yearly CP12 gas safety certificate in place. Always write "CP12 gas safety certificate", never "CP12" alone.
- **older-relatives**: written for someone who looks after an older parent or relative, focused on peace of mind and a warm, safe home.

If the CSV has a group not listed above, fit the opening and example to what its name plainly means, and mention it in the summary.

You may update the `Preview:` line to match the new opening, as long as it stays 90 characters or fewer.

## 4. Brand checks for every version

Before saving, check each version against `brand/voice.md` and `.claude/rules/emails.md`:

- Plain English.
- No pressure words (hurry, act now, don't miss out, limited time, countdowns, slots left).
- No scare words or carbon monoxide scare stories, and no claim that a boiler is dangerous. Keep it calm and reassuring, especially for older-relatives.
- No prices or price figures. Prices are agreed in a written quote.
- No same-day, 24/7 or emergency-anytime promises.
- No religious greetings such as "Merry Christmas".
- The review is still copied word for word from the approved list, unchanged from the original email.

If a change would break any of these, rewrite the change, not the rule.

## 5. Save

Save each version to `<campaign folder>/groups/<group>/emails/<same file name>`. Overwrite an earlier copy if one exists. Never change the original emails.

## 6. Reply

Reply with a short summary only:

- How many files you wrote (and for how many groups and emails).
- One example opening line per group.

Don't paste whole emails into the reply.
