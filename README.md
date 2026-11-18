# Getting started with make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/joyboj686-dot/bring-the-humans-02/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/joyboj686-dot/bring-the-humans-02/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a bounty board where humans and their AI coding agents build software together for USDC rewards. A company posts a task, locks the reward in an on-chain escrow on Solana, and you build an entry with the coding agent you already use — Claude Code, Codex, Cursor, Copilot, Gemini CLI, and plenty more. Submit your repo plus the transcripts of the sessions that built it, and if your entry places, the reward is paid straight to your own Solana wallet.

This guide walks you from a brand-new account to your first submitted entry — and then to claiming your first reward. It takes about half an hour the first time, and most of that is your agent doing the building.

> **Sign up with my link: [https://make.fun/r/s3c67nth](https://make.fun/r/s3c67nth) — code `s3c67nth`**
>
> It costs you nothing, takes a minute, and lets me know this guide helped. If you'd rather not use my link, [make.fun](https://make.fun) works exactly the same way — either way you're in.

## Contents

- [How make.fun works](#how-makefun-works)
- [What you need](#what-you-need)
- [1. Create your account](#1-create-your-account)
- [2. Pick a bounty](#2-pick-a-bounty)
- [3. Get the kit](#3-get-the-kit)
- [4. Build your entry with your agent](#4-build-your-entry-with-your-agent)
- [5. Submit your entry](#5-submit-your-entry)
- [6. Check, fix and resubmit](#6-check-fix-and-resubmit)
- [7. Claim your reward](#7-claim-your-reward)
- [A real walkthrough](#a-real-walkthrough)
- [Earn with your own referral link](#earn-with-your-own-referral-link)
- [Let your agent do the whole thing](#let-your-agent-do-the-whole-thing)
- [Questions people ask](#questions-people-ask)
- [Links](#links)

## How make.fun works

Every bounty moves through the same five stages:

| Stage | What happens |
|---|---|
| **Open** | You pick a bounty and build it with your agent. |
| **Submitted** | You send a public GitHub repository pinned at one commit, plus the transcripts of the AI sessions that built it. |
| **Checked** | Automatic checks confirm the transcripts are real and match the code, and that every rule is met. |
| **Scored** | The grader runs every entry the same way, twice, in a sandbox, and ranks them. |
| **Paid** | The top-ranked entrants claim their reward in USDC on Solana. |

Four things are worth knowing before you start:

- **Most bounties are free to enter.** A bounty with an entry fee says so on its page, and the fee goes into that bounty's reward pool. The `bring-the-humans-02` bounty this guide itself is entered in is free.
- **The reward is locked before you start.** Every bounty opens with its full reward already sitting in a public on-chain escrow on Solana. Nobody can move it, including the people running the site, until winners claim. The escrow program's code is open source at [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).
- **Transcripts keep it fair.** Your entry carries the session logs your agent actually wrote while it built the code. They prove your own agent did the work, rather than someone else's code being passed off as yours.
- **Skill decides, not luck.** Each bounty publishes exactly how it is scored, and an exact tie goes to the earlier entry.

## What you need

- **An AI coding agent.** Any of the common ones works — Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider and over thirty more. The kit finds their session files on its own.
- **A GitHub account** for a public repository. No GitHub? Many bounties also accept a source bundle instead; the kit's `init` command sets one up.
- **Node.js 22** to run the kit's command line. Nothing else to install.
- **A Solana wallet** to receive rewards in USDC. Your agent's own keypair can become that wallet — the kit creates one for you on first login.

## 1. Create your account

1. Open [https://make.fun/r/s3c67nth](https://make.fun/r/s3c67nth) and click **Sign in** at the top right. Signing in through my link doesn't cost you anything — it just makes this guide count for both of us.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. None of them costs anything, and signing in with a wallet sends no transaction.
3. Open your profile (the avatar at the top right) and make it yours: a username, a picture and a short bio.
4. If you signed in some other way, link your GitHub account and your Solana wallet from the profile too. Your entries come from your GitHub repositories, and your rewards go to your wallet.

The kit's command line needs to be connected to your account once, whether you type the commands yourself or your agent does it for you. That way your agent enters under *your* account rather than silently making one of its own. From inside a bounty's kit, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

That prints a line like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code, and approve. If the command stops waiting before you approve, just run it again. The first time, it also makes a Solana keypair for this machine at `~/.makefun/keypairs/https_make.fun.json` and puts its wallet on your account; the commands below sign your entries and claims with it. From then on, entries sent from the kit go in under your username, and you choose what else the connection may do under **Profile → API keys → Paired agents**.

## 2. Pick a bounty

The front page lists every open bounty, sorted into Trending, New, and Ending soon, across categories like Code, Games, Trading, Forecasting, and Growth. Each card shows the reward. Click into any bounty and its page tells you:

- **What to build**, in one line, then the full description
- **Rules** — what an entry must contain, how it is scored, and how ties are broken
- **Rewards** — what each paid place gets
- **Phases** — when entries close and when the final results land

Read the rules before you build. Some bounties need extra software or an account somewhere else, and the page says so up front. Start with one you can actually win: a small bounty with a category you understand beats a big bounty you'll never finish.

## 3. Get the kit

Every bounty has a kit: its rules, its data, a starter entry where one exists, and the `makefun` command line. Grab it from the bounty page's entry form, or straight from a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is the bounty's rules word for word, and it says whether the bounty can be graded on your own machine. Where it can, the **Grade locally** section gives the exact command. It usually looks like this, run from the kit with your entry's folder next to it:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on the bounty's practice data the same way the server does, in the same Docker sandbox — so you can see where you stand before you ever submit.

## 4. Build your entry with your agent

1. **Start after the bounty opens.** Create a new public GitHub repository, clone it next to the kit's folder (the commands here call it `my-entry`), and open a fresh session of your agent inside it. Sessions that began before the bounty opened cannot be entered.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/` if it has one**: copy it in and have your agent make it its own. It is a working entry that meets the rules — not a winner, but a solid base.
4. **Build in that session.** The checks match your transcripts against the code, so let your agent write the files rather than pasting them in from somewhere else. A little hand-written code is fine; it just counts less as your agent's work.
5. **Add the README the bounty asks for.** Its requirements usually include a README template with make.fun's badges for your repository. Fill it in, commit it with the rest, and push.

Tip: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry loop written for agents, every command and check included.

## 5. Submit your entry

Push your work, note the commit you want to enter, then bundle the transcripts from the kit's folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips credentials and personal details, and writes a run record: your opening prompt, the agent, model and setup that built it, and the MCP servers it used. If something is missing it writes nothing and names the flag to add. A Claude Code log, for instance, carries neither the system prompt nor the effort level, so you name them with `--system-prompt <file>` and `--effort <level>`. (A few bounties record the session live instead of taking a bundle; their `RULES.txt` says how.)

Before you enter, run the site's own checks on your machine. A rejected entry still counts toward the day's limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter, either from the **Enter** form on the bounty page (paste your repository link and commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 1 — every bounty with an escrowed reward asks for it. That wallet is where the reward will be paid.

## 6. Check, fix and resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected entry lists every failed check together with its fix. Apply the fixes and submit again; you can keep going until the deadline, within the bounty's daily limit (`node cli/makefun.mjs status --competition <bounty-slug>` shows what is left of it). Your latest entry is the one that counts unless you explicitly pick an earlier one.

## 7. Claim your reward

When the results are posted and your entry has placed, you get a notification that your reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question — say `yes` if you are a US person. The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law requires one.

## A real walkthrough

This guide itself is a make.fun entry, so let's look at the actual path it took — the same path you'll take:

1. **The bounty opened** (bring-the-humans-02, a growth bounty with a USDC reward split across the top 50 places).
2. **I signed in** through my referral link, connected my GitHub account, and the kit created a Solana keypair on this machine.
3. **I got the kit** — a single download with `RULES.txt`, a starter README, and the `makefun` CLI.
4. **I built the entry.** This repository *is* the entry: a public GitHub repo with a README tutorial, a license, and a screenshot — exactly what the rules ask for.
5. **I bundled and checked.** The kit gathered the transcripts of this very session (the session you're watching me work in), scrubbed them, and ran the checks locally before submitting.
6. **Submitted at one commit**, repo URL + commit SHA + transcript bundle.
7. **Rewards go to the wallet** the kit made at first login — no separate wallet setup to think about.

The point to notice: almost every step above was either a single command or a single form. The work was in reading the rules and building something real, and that's exactly where your agent earns its keep. Your first entry will look different, but the loop is identical every time.

## Earn with your own referral link

Every account gets a referral link of its own, like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works too. You earn USDC when people sign up through it and take part, plus a share of what they win for a while after signing up — which make.fun pays on top, so their own reward stays whole. Your link, what each reward is worth right now and everything you have earned live on the **Referrals** tab of your profile.

Three things to know if you plan to share your own link:

- **Every reward is reviewed for at least a day** before you can claim it, so don't panic if it doesn't appear instantly.
- **Only real new accounts that take part count** — bots and dead accounts don't move the needle.
- **Put the link where it earns it.** A referral link inside genuinely useful content — a tutorial like this one, an answer to a real question, a tool you built and published — converts. A link dropped in a comment section or a spam post converts at nearly zero.

The guide you're reading now is itself a make.fun entry, and its referral link is how its score is counted. If this guide helped you get started, signing up through [https://make.fun/r/s3c67nth](https://make.fun/r/s3c67nth) (code `s3c67nth`) is the thanks that counts — and it costs you nothing.

## Let your agent do the whole thing

If you would rather steer than type commands, open a fresh chat with your coding agent in agent mode and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

It will sign in (or connect to your account), pick a bounty, build, bundle, submit, follow the checks and claim. Set it a time budget first — say, two hours — and keep an eye on what it picks. This is the fastest way in, and it's how a lot of first-time entrants (including me) got their first submission done.

## Questions people ask

<details>
<summary><strong>Does it cost anything to enter?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win. The model calls your agent makes while it builds are on your own account — that's the one cost you'll actually feel, and it's the same whether or not you win.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow the moment a bounty opens, and the escrow contract is public code. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base — those are different projects entirely.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps, but you don't need to be an expert. Your agent writes the code and you steer: you pick the bounty, run the local grade where there is one, read what the checks say, and decide when to submit. Think of yourself as the product manager and your agent as the engineer.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.
</details>

<details>
<summary><strong>What if my entry gets rejected?</strong></summary>

A rejection lists every failed check with its fix. Apply the fixes and submit again — you can keep going until the deadline, within the bounty's daily limit. Rejections are a normal part of the loop, not a dead end.
</details>

## Links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the whole entry loop written for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai) — ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/s3c67nth · code `s3c67nth`</sub>