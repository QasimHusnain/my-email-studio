---
name: personalize
description: Write a version of each campaign email for every lead group in leads/leads.csv. Use when a campaign's emails are final and you want one copy per group.
argument-hint: "[campaign folder]"
context: fork
allowed-tools: Read, Glob, Grep, Write
---

# Personalize a campaign for each lead group

Campaign folder: $ARGUMENTS

You are making one copy of each finished campaign email for every lead group, so each group reads an opening and an example that fit their life. The originals are final and approved. That is why almost everything is copied exactly and only two small parts change: anything else you touch is a part nobody has checked against the brand.

This runs in a forked context. The main chat only sees your final reply, so do all the reading, writing and checking here and return a short summary.

## Step 1: Check the input

1. **No campaign folder given** (the line above is empty): Glob `campaigns/*/` and stop. Reply with the list of campaign folders that exist and ask which one to use. Write nothing.
2. **Folder doesn't exist**: stop, say so, and list the campaign folders that do exist. Write nothing.
3. **No emails**: Glob `<campaign folder>/emails/*.md`. If there is no `emails/` folder, or it holds no `.md` files, stop and say which one it is ("no emails/ folder" or "emails/ folder is empty"). Write nothing.

Accept the folder as `campaigns/<name>`, `campaigns/<name>/` or just `<name>`; they all mean `campaigns/<name>`.

## Step 2: Read what you need

Read these before writing anything:

1. `brand/voice.md`: the voice, words to avoid, claims we must never make, and the approved reviews.
2. `.claude/rules/emails.md`: the layout every email file must follow.
3. `leads/leads.csv`: take the distinct values of the `group` column. Use only that column. Never copy a name or email address into an email, since these copies are templates filled in later by `{{first_name}}`.
   - If the file has no `group` column, or no group values, stop and say so. Write nothing.
4. Every file in `<campaign folder>/emails/*.md`. Read only the top-level `emails/` folder. Never use an existing `groups/` copy as a source, because it may be out of date.
5. Glob `<campaign folder>/groups/*/emails/*.md` and note which copies already exist, so you can count the files you replace.

Check each original's review against the approved list in `brand/voice.md`. If an original uses Mark's review (it's about an emergency leak, not a service) or a review that isn't on the list word for word, don't write copies of that email. Report it in the summary, because the fix belongs in the original, not in the copies.

## Step 3: Decide what changes and what doesn't

### Copied exactly, character for character

- The `Subject:` line.
- The `Preview:` line.
- The greeting (`Hi {{first_name}},`) and every `{{...}}` placeholder.
- The order of paragraphs and every paragraph you aren't changing.
- The review line, with its quote marks, dash and first name.
- `[Book your service](https://brightwellheating.example/contact/) · Call or WhatsApp 07700900123`
- The sign-off (`Thanks,` / `Daniel at Brightwell Heating`).
- The three footer links and the two-line footer.

The easiest way to get this right is to start from the original text and edit it, not to retype the email.

### The two things you change

1. **The opening line**: the first paragraph after the greeting. Rewrite it so it speaks to the group, while keeping its job (for example, Daniel introducing himself and saying why he's writing).
2. **One example**: one sentence or short paragraph in the body that pictures the reader's situation. Pick the one that most clearly describes a particular household. If a paragraph already lists several kinds of reader ("children's baths… an older relative… a landlord"), that is the one to change, narrowed to this group. Change one example only, even if others could be tuned too.

Keep each change about the same length as what it replaces, in the same calm voice, and a natural fit with the sentences around it.

### How to fit each group

- **families**: a busy home with young children. For example, no hot water for bath time on a cold January morning.
- **older-relatives**: keeping a parent or grandparent warm and comfortable at home through the winter. Write to the person who looks after them, with a focus on comfort and peace of mind.
- **landlords**: keeping tenants warm, and the yearly CP12 gas safety certificate up to date, booked alongside the service.
- **Any other group** in the CSV: fit the opening and example to what the group name plainly means, using the same care, and mention the group in the summary so someone can review it. If the name gives no clear meaning, still write the copy with a light, general change and say so in the summary.

## Step 4: Brand rules for every change

Every change must still follow `brand/voice.md`:

- Plain English, calm and never pushy. An honest picture of a cold morning is fine; fear is not.
- No pressure words: hurry, act now, don't miss out, limited time, countdowns, "only X slots left".
- No prices, price figures, discounts or deals. Prices are agreed in a written quote.
- No same-day, 24/7, instant or emergency-anytime promises.
- No scare words (deadly, killer, silent killer, explosion), no carbon monoxide stories, and no claim that a boiler is dangerous. Take extra care with older-relatives: reassure, don't worry.
- Always write "CP12 gas safety certificate" in full. Never "CP12" alone.
- No religious greetings such as "Merry Christmas". Say "the holidays" or "the new year".
- No new claims, numbers, accreditations or reviews. Use only the review already in the original, and never Mark's in a service email.

If a change breaks a rule, rewrite the change. Never bend the rule.

## Step 5: Check each copy before saving it

For every copy, compare it with the original line by line before you call Write:

1. **Subject**: identical.
2. **Preview**: identical.
3. **Link and phone line**: identical, and still the only link in the body.
4. **Review**: identical to the original and on the approved list word for word.
5. **Sign-off and footer**: identical, from `Thanks,` to the last line.
6. **Only two changes**: apart from the opening line and the one example, every line matches the original. Count the paragraphs that differ: it should be two (or one, if the example sits in the opening paragraph).
7. **Brand check**: the changed parts pass every rule in Step 4, including "CP12 gas safety certificate" in full.
8. **Fits the group**: the opening and example clearly speak to this group and not to another one.

If any check fails, fix the copy and check again. Save only a copy that passes all eight.

## Step 6: Save

Write each copy to `<campaign folder>/groups/<group>/emails/<same file name>`, using the group name exactly as it appears in the CSV. Overwrite a copy that is already there. Never change or save over the originals in `emails/`.

After saving everything, run one last check across the saved copies with Grep on `<campaign folder>/groups/`:

- `CP12` not followed by ` gas safety certificate` (should find nothing).
- The pressure, price, promise and scare words from Step 4 (should find nothing that isn't already in the original).
- `Subject:` and `Book your service` once in every file.

Fix and re-save any file this turns up.

## Step 7: Reply

Reply with a short summary only. Don't paste whole emails.

- How many files you wrote (groups × emails), and how many of them replaced existing copies.
- One example opening line per group.
- Only if they apply: any group not in the usual three, any email skipped and why.

For example:

```
Wrote 12 files (3 groups × 4 emails); 12 replaced existing copies.

- families: "It's Daniel from Brightwell Heating. With little ones at home, a warm house matters more than ever in winter."
- older-relatives: "It's Daniel from Brightwell Heating. If you look after a parent or grandparent, keeping their home warm this winter is a big part of that."
- landlords: "It's Daniel from Brightwell Heating. Winter is when tenants rely on their heating most, so now is a good time to get it checked."
```
