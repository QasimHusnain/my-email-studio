# Skills I chose

Numbers checked on 6 November 2026. Stars are for the whole GitHub repository, and installs are for the single skill on skills.sh. Nothing has been installed yet.

## Use these

**For writing campaigns: `anthropics/knowledge-work-plugins`**
- Skills: `campaign-plan` (3.4K installs) for the brief, and `email-sequence` (3K installs) for the emails.
- Published by Anthropic, the company behind Claude. The repository has about 35K stars and is actively maintained.
- All three security audits on skills.sh pass (Gen Agent Trust Hub, Socket, Snyk).
- Their install counts are much lower than some other results, but the publisher and the audits are stronger.

**For sending email: `resend/resend-skills`**
- Skill: `resend` (24.3K installs).
- Published by Resend, the email service itself, so it should stay in step with their product.
- The repository has only 211 stars, which is low. I still trust it because the same organization runs `resend/react-email` (about 19.9K stars).
- One warning to know about: Snyk rates it medium risk. Emails received from outside senders could contain hidden instructions aimed at the AI. This matters only if we use the skill to read incoming email, so sending alone avoids most of it.

## Avoid these

**`caffeinelabs/skills@extension-email-marketing`**
- It is the first result for "email marketing" with 30.2K installs, but its repository has just 2 stars. A big gap like that means the install number alone proves little.
- It only works with apps built on Caffeine AI, and its page promotes that product with tracking links. It wouldn't help this studio.

**`membranedev/application-skills@resend`**
- It has "resend" in its name but isn't from Resend (133 installs, 271 stars).
- It sends your Resend login and every email action through another company's tool, Membrane. That means trusting a second company with access to your email account.
- Snyk scored its email-content risk as high (0.80, against 0.30 for the official skill), and Socket also flagged it as suspicious. Neither audit confirmed any malware. The official skill does the same job without the middleman.

## How I decided

Check these three things before installing any skill:

1. **Who publishes it?** Prefer the company that makes the product. Don't trust a name alone: anyone can put "resend" in a skill's name, so check the owner in `owner/repo`.
2. **Do the numbers agree?** Compare installs with GitHub stars, and check how old the repository is and when it was last updated. High installs with almost no stars is a warning sign.
3. **What does it actually do?** Read the security audits on its skills.sh page, including why any warning was given, then read the skill's `SKILL.md`. Look for logins passed to third parties, unknown tools it makes you run, and promotion of a product.
