# Email Studio: Brightwell Heating

**Author:** Qasim Husnain
**Course:** Email Studio Lab (assignment FDE-102), The AI Agent Factory FDE Fast Track
**Progress:** All 15 steps done, including the stretch step

This is an email marketing studio run with Claude Code, built without writing code by hand. Claude plans a campaign, writes the emails, personalizes them for each group of customers, and sends them safely to a practice email server. Every send passes through a guard that blocks real people.

## About the brand: a stand-in for a real business

**Brightwell Heating is a fake name for a real business.**

I built this studio for a real local heating company in Bradford, West Yorkshire, one I provide services to. To protect that business's privacy, everything that could identify it has been replaced with made-up details before publishing:

| Detail | What this repo uses |
|---|---|
| Company name | Brightwell Heating Ltd |
| Engineer | Daniel (Daniel Hart) |
| Website | `brightwellheating.example` (a domain that can never be registered) |
| Phone and WhatsApp | 07700900123 (a range Ofcom keeps for TV and drama, so it never connects) |
| Gas Safe registration | No. 000000 |
| Google reviews | Made-up reviews and reviewer names |

The working version with the real business details is kept privately on my own machine. This public repo shares only the fake identity. The work itself is the same in both: the brand voice, campaign plan, rules, skill, guard and review script. The city (Bradford, West Yorkshire) is real but is not private information.

All customers in `leads/leads.csv` are made up and use `@example.com` addresses, which can never receive mail.

## How this repo maps to the assignment

The assignment's examples use the sample brand Kettlebird Tea. The course allows your own brand, so names differ from the assignment's examples:

| Assignment example | This repo |
|---|---|
| Kettlebird Tea, The Steep Club | Brightwell Heating, yearly boiler service and CP12 gas safety certificate |
| `campaigns/steep-club-launch/` | `campaigns/boiler-service-reminder/` (campaign name: Warm Home Winter Check) |
| Groups `tea-curious`, `gift-buyers`, `daily-drinkers` | Groups `families`, `landlords`, `older-relatives` |
| Practice launch tags `[tea-curious]` and so on | `[families]`, `[landlords]`, `[older-relatives]` |

## Steps completed

| Step | What it covers | Where to look |
|---|---|---|
| 1. Look before you build | Researching existing skills and plugins before building, and deciding which to trust | `notes/skills-i-chose.md` |
| 2. Set up your studio | Folders and the studio's rules | `CLAUDE.md`, `brand/`, `leads/`, `campaigns/`, `notes/` |
| 3. Your brand voice | How the brand sounds, the claims it must never make, and the approved reviews | `brand/voice.md` (loaded through `@brand/voice.md` in `CLAUDE.md`) |
| 4. Plan the campaign | Goals, audience, schedule and checkpoints | `campaigns/boiler-service-reminder/plan.md` |
| 5. Practice leads | 24 made-up customers in three groups, and rules that load only for this folder | `leads/leads.csv`, `leads/CLAUDE.md` |
| 6. Email rule and sequence | A rule for every email file, and four emails | `.claude/rules/emails.md`, `campaigns/boiler-service-reminder/emails/` |
| 7. Fresh-eyes review | Brand review by a session that didn't write the emails, then fixes | Git history |
| 8. Your own skill: personalize | A skill that writes one copy of each email per lead group | `.claude/skills/personalize/SKILL.md`, `campaigns/boiler-service-reminder/groups/` |
| 9. Connect Resend in practice mode | Resend MCP server pointed at a local practice server by default | `.mcp.json` |
| 10. The safety guard | PreToolUse hook that blocks unsafe sends | `.claude/hooks/send-guard.mjs`, `.claude/settings.json` |
| 11. Practice launch | Each group's email 01 sent to `delivered@resend.dev` | Practice server (not stored in the repo) |
| 12. Resume and fork | Two subject line angles worked on in parallel, then a handoff note | `notes/angle-a.md`, `notes/angle-b.md`, `notes/handoff.md` |
| 13. Automatic review | Headless Claude review that returns JSON matching a schema | `scripts/review.mjs`, `review/findings.schema.json`, `review/findings.json`, `campaigns/test-bad/` |
| 14. Package your studio kit | The reusable parts (personalize skill, send guard, email reviewer agent, review script) packaged as a plugin, then tested on a second made-up client, Rise & Crumb bakery | `studio-kit/` and `client-two/`, two separate git repos kept next to this one (see below) |
| 15. What customers love (stretch) | 120 made-up practice reviews read by a subagent that writes a scratchpad as it goes, so findings survive `/compact` | `notes/reviews/`, `notes/scratchpad.md` |

### Step 14: studio-kit and client-two

Step 14 builds two folders next to this studio, each with its own git repo:

- `studio-kit/` (https://github.com/QasimHusnain/studio-kit): the plugin. It holds only reusable parts and uses `${CLAUDE_PLUGIN_ROOT}` for paths. `claude plugin validate` passes, and it mentions no client by name.
- `client-two/` (https://github.com/QasimHusnain/client-two): a second made-up client, Rise & Crumb bakery, with its own brand voice, leads, email rule and campaign. It is run with `claude --plugin-dir ../studio-kit`.

Brand voice, leads, campaigns and rules stay in each client's own repo, because they belong to that client. Only the tools travel in the plugin.

## What's inside

```
my-email-studio/
├── CLAUDE.md                  the studio's rules (always loaded)
├── brand/voice.md             brand voice, claims to avoid, approved reviews
├── leads/
│   ├── leads.csv              24 made-up practice customers (@example.com)
│   └── CLAUDE.md              rules that load only for files in leads/
├── campaigns/
│   ├── boiler-service-reminder/
│   │   ├── plan.md            the campaign plan
│   │   ├── emails/            4 emails
│   │   └── groups/            a copy of each email for each lead group
│   └── test-bad/              a deliberately bad email to prove the review works
├── notes/                     skills research, subject line angles, handoff note,
│                              practice reviews and the review scratchpad
├── .claude/
│   ├── settings.json          shared settings: plugin and the send-guard hook
│   ├── rules/emails.md        rules for campaigns/**/emails/*.md
│   ├── skills/personalize/    the personalize skill (runs in a forked context)
│   └── hooks/send-guard.mjs   blocks unsafe sends
├── .mcp.json                  Resend connection, practice mode by default
├── review/                    findings schema and latest findings
└── scripts/review.mjs         headless automatic review
```

## Send safety

Nothing in this studio can email a real customer. There are three separate layers:

1. **Practice mode by default.** `.mcp.json` uses `${RESEND_API_KEY:-re_practice}` and `${RESEND_BASE_URL:-http://localhost:4000}`, so without real values set in the terminal, every send goes to a local practice server. Real keys are never stored in a file.
2. **Fake leads only.** Every address in `leads/leads.csv` ends in `@example.com`, a reserved domain that can never receive mail.
3. **The send guard.** `.claude/hooks/send-guard.mjs` runs before every Resend tool call. It blocks (exit code 2) any recipient other than `@resend.dev` test addresses or my own address, and always blocks tools that can reach a whole list, such as broadcasts.

My own address (`MY_EMAIL`) lives in `.claude/settings.local.json`, which is gitignored and not in this repo.

## Try it

You need Node.js 22 or newer, Git and Claude Code.

```
# Terminal 1: start the practice email server on http://localhost:4000
npx emulate@0.12.3 start --service resend

# Terminal 2: open the studio
claude

# Run the automatic review on a campaign
node scripts/review.mjs campaigns/test-bad                  # exits 1: the bad email has high-severity problems
node scripts/review.mjs campaigns/boiler-service-reminder   # writes review/findings.json
```

The review script runs Claude Code headless, saves the findings to `review/findings.json`, and exits with code 1 if any finding is high severity.
