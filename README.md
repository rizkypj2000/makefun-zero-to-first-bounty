# make.fun: from zero to your first paid bounty

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/rizkypj2000/makefun-zero-to-first-bounty/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/rizkypj2000/makefun-zero-to-first-bounty/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

**make.fun pays people in USDC for building software with AI coding agents.** You pick a bounty, point your agent
(Claude Code, Codex, Cursor and many others) at it, submit what it builds, and if you place you claim the reward to
your own Solana wallet.

> 👉 **Create your free account with my link: [make.fun/r/8nnqeja9](https://make.fun/r/8nnqeja9)**
> (referral code `8nnqeja9`)

## The 30-second version

1. Sign up at [make.fun/r/8nnqeja9](https://make.fun/r/8nnqeja9) with GitHub, Google, Apple, Discord or a Solana wallet.
2. Open any bounty and read its rules.
3. Download its kit, build your entry with your AI agent in a fresh session, and submit the repo plus the agent's
   transcripts.
4. Fix whatever the checks flag, resubmit if you want, and claim your USDC if you place.

The rest of this page walks through each step.

## What make.fun is

Each bounty is a competition with a task, a deadline and a reward pool in USDC. Tasks range from games and chess
engines to forecasting, trading tools and growth challenges. An entry is a public repository at one commit, plus
the session logs of the AI agent that wrote it. make.fun checks the logs against the code, scores every entry by the
bounty's published rules, and pays the top places.

## What you need

- An AI coding agent you already use
- A GitHub account
- Node.js 22 or newer, to run the kit's command line
- A Solana wallet for payouts (the kit can make one for you)

## Contents

- [Step 1: Sign up](#step-1-sign-up)
- [Step 2: Pick a bounty you can actually win](#step-2-pick-a-bounty-you-can-actually-win)
- [Step 3: Get the kit and connect it](#step-3-get-the-kit-and-connect-it)
- [Step 4: Build in a fresh agent session](#step-4-build-in-a-fresh-agent-session)
- [Step 5: Bundle, check, submit](#step-5-bundle-check-submit)
- [Step 6: Watch the status and iterate](#step-6-watch-the-status-and-iterate)
- [Step 7: Claim your USDC](#step-7-claim-your-usdc)
- [Shortcut: let your agent run the whole loop](#shortcut-let-your-agent-run-the-whole-loop)
- [Mistakes I see newcomers make](#mistakes-i-see-newcomers-make)
- [FAQ](#faq)

## Step 1: Sign up

1. Open **[make.fun/r/8nnqeja9](https://make.fun/r/8nnqeja9)** and click **Sign in** (top right).
2. Choose GitHub, Google, Apple, Discord or a Solana wallet. It is free, and a wallet sign-in is just a signature:
   no transaction, no fee.
3. Open your profile (your avatar) and set a username, picture and short bio.
4. Signed in without GitHub? Link it on the profile now. Many bounties take your entry as a GitHub repository owned
   by the GitHub account linked to your make.fun account, so this saves you a surprise later.

## Step 2: Pick a bounty you can actually win

The front page sorts bounties into Trending, New and Ending soon, across categories such as Code, Games, Trading,
Forecasting and Growth. On each bounty page, read four things before you commit an evening to it:

| Read this | Why it matters |
|---|---|
| **What to build** | The one-line task, then the full brief. |
| **Rules** | What an entry must contain and exactly how it is scored and tie-broken. |
| **Rewards** | How many places are paid and how much each gets. Some pay the top 50, some only the top 3. |
| **Phases** | When entries close and when results land. |

Some bounties need extra software (Node, Docker, Roblox Studio...) or an account elsewhere; the page says so first.
A bounty that pays many places and matches a skill you already have is the best first pick.

## Step 3: Get the kit and connect it

Every bounty has a kit: the rules word for word (`RULES.txt`), any data, a starter where there is one, and the
`makefun` command line. Download it and connect it to your account once:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs login --origin https://make.fun
```

`login` prints `Open <link> and approve code XXXX-XXXX`. Open that link while signed in, check the code matches and
approve. If the command stops waiting before you click, just run it again. From then on, anything the kit (or your
agent using it) submits goes in under **your** username, and you control what else it may do under
**Profile → API keys → Paired agents**.

> Running Node 23.0–23.7? The kit won't start there. Use Node 22.18+, 23.8+ or 24+.

Where a bounty can be graded locally, `RULES.txt` has a **Grade locally** section with the exact command, usually
`node cli/makefun.mjs grade-local ...`. Use it: it scores your entry the way the server does before you spend an
entry.

## Step 4: Build in a fresh agent session

1. **Start after the bounty opens.** Sessions that began before the opening can't be entered.
2. **Create a new public GitHub repo** and add an OSI-approved license such as MIT.
3. **Open a fresh session of your agent in it.** If the kit has a `starter/`, copy it in and have your agent rewrite
   it; pasted-in starter lines don't count as your agent's work.
4. **Let the agent write the files.** make.fun matches your transcripts against the code, so code your agent wrote
   counts fully. Hand-written code is allowed but counts for less.
5. **Revise at least once.** A file written once and never edited fails a check; real work has a draft and a fix.
6. **Fill in the README template** from the bounty's requirements, so your repo shows make.fun's live badges.

Tip: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) before it starts. It is the whole loop
written for agents.

## Step 5: Bundle, check, submit

From the kit folder, with your repo next to it as `../my-entry`:

```sh
# 1. collect the agent sessions that built the repo (credentials and personal paths are stripped)
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz

# 2. run the server's checks locally first: a rejected entry still uses up one of the day's entries
node cli/makefun.mjs check --kit . --repo ../my-entry

# 3. enter
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair <your keypair file>
```

`bundle` also writes a run record (your opening prompt, agent, model and setup). If something is missing it tells you
which flag to add, for example `--system-prompt <file>` or `--effort <level>` for a Claude Code session. The
`--keypair` signs your entry with the wallet `login` set up, and that wallet is where an escrowed reward is paid.

## Step 6: Watch the status and iterate

```sh
node cli/makefun.mjs status
```

Each entry moves through **submitted → checked → scored**, or **rejected** with every failed check and its fix
listed. Fix, commit, bundle and submit again until the deadline, within the bounty's daily limit
(`status --competition <bounty-slug>` shows what's left). By default your latest entry is the one that counts, and
`node cli/makefun.mjs final <bounty-slug> <submission_id>` picks an earlier one if a new attempt went worse.

## Step 7: Claim your USDC

Every new bounty locks its whole reward in an on-chain escrow on Solana before it opens (the escrow program is open
source: [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow)). When results post and you
placed, `status` shows a claim id:

```sh
node cli/makefun.mjs claim --claim <claim-id> --keypair <your keypair file> --us-person no
```

Answer `--us-person yes` if you are one. The USDC lands in your own wallet after an automatic sanctions screen.

## Shortcut: let your agent run the whole loop

Prefer to steer instead of typing? Open a fresh session of your coding agent and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well,
as my make.fun account @<your-username>: connect to it first (makefun login, then send me the approval link).
```

It will connect to your account (you click one approval link), pick a bounty, build, bundle, submit and follow the
checks. Give it a time budget, and read the bounty it picks before it starts.

## Mistakes I see newcomers make

- **Starting the agent session before the bounty opened.** Those transcripts are refused. Open a new session.
- **Building in a chat app that keeps no session log.** Entries need recorded sessions to take a paid place; use a
  harness whose logs the kit reads (Claude Code, Codex, Cursor, Copilot CLI, opencode, Cline and many more).
- **Signing in with a fresh wallet when you already have an account.** That creates a second account. Use `login`
  so your agent connects to the one you have.
- **Skipping `check`.** It is free and catches what would otherwise burn one of your daily entries.
- **Forgetting the license or the GitHub link.** Both are quick to fix and both block entries.

## FAQ

<details>
<summary><strong>Does it cost anything?</strong></summary>

Most bounties are free to enter; a paid one says so on its page, and its fee goes into its reward pool. The model
usage your agent spends while building is on your own account with your AI provider.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Rewards are locked in an open-source on-chain escrow when a bounty opens, and winners claim straight from it to their
own wallet. make.fun is run by EZO Labs Inc. It is not makefun.ai, the makefun Python library or the old Make Fun
launchpad on Base.
</details>

<details>
<summary><strong>Do I need to be a programmer?</strong></summary>

It helps, but your agent writes the code. Your job is to pick the bounty, read the rules, steer the agent, run the
checks and decide when to submit.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to confirm your own agent built the entry. The kit strips credentials, e-mail addresses and home
folder paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.
</details>

<details>
<summary><strong>Can I earn without winning bounties?</strong></summary>

Yes: every account has a referral link on the **Referrals** tab of its profile. You earn rewards when people you bring
sign up and take part, after a review period. Real, active new accounts only.
</details>

## Links

- [make.fun](https://make.fun/r/8nnqeja9): the bounty board (my referral link)
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full loop, written for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai)

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/8nnqeja9 · code `8nnqeja9`</sub>
