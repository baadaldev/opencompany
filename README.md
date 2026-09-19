<p align="center">
  <picture>
    <source srcset="docs/gitbooks/.gitbook/assets/opencompany-hero.gif" type="image/gif" />
    <img src="docs/gitbooks/.gitbook/assets/opencompany-hero.png" alt="OpenCompany: run an entire company with a headcount of one" />
  </picture>
</p>

<h1 align="center">OpenCompany</h1>

<p align="center">
  <strong>A hive mind that runs your company. Headcount: one.</strong>
</p>

<p align="center">
  OpenCompany is the operating layer for one-person businesses powered by
  agents. Not a team of agents taking turns, but a hive mind: a roster that
  deliberates like a colony, converges on decisions it can explain, and does
  the work of every function around the clock. You bring the vision and the
  judgment calls. The hive does the rest.
</p>

<p align="center">
  <a href="https://github.com/tinyhumansai/opencompany/blob/main/LICENSE"><img src="https://img.shields.io/github/license/tinyhumansai/opencompany?style=flat-square" alt="License: GPL-3.0" /></a>
  <a href="https://github.com/tinyhumansai/opencompany/stargazers"><img src="https://img.shields.io/github/stars/tinyhumansai/opencompany?style=flat-square" alt="GitHub stars" /></a>
  <a href="https://github.com/tinyhumansai/opencompany/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22"><img src="https://img.shields.io/github/issues/tinyhumansai/opencompany/good%20first%20issue?style=flat-square&label=good%20first%20issues&color=7057ff" alt="Good first issues" /></a>
  <a href="https://github.com/tinyhumansai/opencompany/commits/main"><img src="https://img.shields.io/github/last-commit/tinyhumansai/opencompany?style=flat-square" alt="Last commit" /></a>
  <img src="https://img.shields.io/badge/status-work%20in%20progress-orange?style=flat-square" alt="Work in progress" />
</p>

<p align="center">
  <a href="https://tinyhumans.ai/opencompany"><img src="https://img.shields.io/badge/website-tinyhumans.ai-2F6EF4?style=flat-square" alt="Website" /></a>
  <a href="https://discord.tinyhumans.ai"><img src="https://img.shields.io/badge/Discord-join-5865F2?style=flat-square&logo=discord&logoColor=white" alt="Discord" /></a>
  <a href="https://x.com/tinyhumansai"><img src="https://img.shields.io/badge/X-@tinyhumansai-000000?style=flat-square&logo=x&logoColor=white" alt="X" /></a>
  <a href="https://www.reddit.com/r/tinyhumansai/"><img src="https://img.shields.io/badge/Reddit-r%2Ftinyhumansai-FF4500?style=flat-square&logo=reddit&logoColor=white" alt="Reddit" /></a>
</p>

> [!WARNING]
> **🚧 Work in progress.** OpenCompany is under active development and moving
> fast. APIs, the CLI, the example harnesses, and the docs will change without
> notice. Explore it, fork it, build on it, but don't depend on anything
> staying put yet. Not production-ready.

---

## Download

The quickest way in is the desktop app. Get it from the website or straight
from the GitHub release:

- **Website**: **[tinyhumans.ai/opencompany](https://tinyhumans.ai/opencompany)**
  — the download button picks the right build for your machine.
- **GitHub Releases**:
  **[github.com/tinyhumansai/opencompany/releases/latest](https://github.com/tinyhumansai/opencompany/releases/latest)**
  — the `.dmg` for Apple Silicon (`aarch64`) or Intel (`x64`) Macs, plus the
  release notes.

Open the `.dmg`, drag OpenCompany into Applications, launch it, and pick a
company. The app updates itself from the same releases page. Prefer to run the
host yourself, in Docker or from source? Jump to the [Quickstart](#quickstart).

## The company of one

For a century, ambition meant headcount. Want to ship a product? Hire engineers.
Want customers? Hire marketers, then sales, then support. Every new capability
was a new payroll line, a new manager, a new quarter of ramp-up.

That tax is gone. OpenCompany turns a single operator into a full org chart.
Scouts, founders, engineers, designers, marketers, lawyers, finance, support and
recruiters, all instantiated as agents, coordinated by one host, working while
you sleep. You stay where humans are irreplaceable: **capital, taste, and the
decisions that actually matter.** Everything else is delegated.

This isn't a chatbot with a to-do list, and it isn't a pipeline of agents
handing a ticket down the line. It's a **company runtime**: a durable host that
stands up a roster of specialized agents, gives each one a clear mandate, seats
them at desks, and lets each desk think as a hive on top of the OpenHuman and
TinyHumans runtimes.

## Not a team of agents. A hive mind.

Most "multi-agent" systems are fan-out: publish a task, wake N agents, collect
the replies, average them somehow. That's a thread pool with a prompt attached.
It has no notion of who is convinced, no way to register a grounded objection,
no reason to stop other than running out of members, and no answer when you ask
afterwards why the group chose what it chose.

Real collectives don't work that way. Ant colonies, honeybee swarms and termite
mounds reach decisions with no leader, no shared memory and far less bandwidth
than five language models sharing a channel, and the mechanisms that let them
have been studied for decades. OpenCompany runs its desks on those mechanisms,
via [tinyhivemind](https://github.com/tinyhumansai/tinyhivemind), the hive-mind
library that grew out of this repo.

### How a desk thinks

A message to a desk with two or more members doesn't pick a responder. It opens
an **episode**: a bounded sequence of rounds in which members deposit marker
lines into the desk's shared transcript, and a pure fold over that transcript
decides who speaks next and when the room is done.

```text
!propose #stage Stage the rollout across three regions.
!support #stage ^1 Staging bounds the blast radius if the migration is wrong.
!object  >3      The regions are not independent, so this bounds nothing.
!commit  #stage
```

- **Stigmergy.** Work leaves a trace in a shared medium, and the trace is the
  stimulus for the next piece of work. Nobody dispatches anybody and no agent
  addresses another. The transcript is the medium; a marker line is a deposit
  in it.
- **Quorum sensing.** An option carries when enough *distinct* members have
  *grounded* support for it inside a window, the way a honeybee swarm settles a
  nest site. Not a majority, not a score to beat. A `!support` with no citation
  counts for nothing, and a late member folds to exactly the same standing as
  one that watched live.
- **Cross-inhibition.** An objection names a *message* and removes its author
  from the supporter set of whatever they were advocating. It doesn't debit the
  option. Subtracting from a score can't break a tie between two equally backed
  options; silencing an advocate can. Honeybees do this too, with stop signals.
- **Pheromone decay.** A trace's pull on the room's attention decays with
  distance in the transcript, so whoever spoke first doesn't hold the floor
  forever. The trace's standing importance is the floor under that decay, which
  is why a proposal nobody has touched for eighty messages still outranks a
  fresh question.
- **Response thresholds.** Every member computes an urge from the salience
  field and its own affinity, and whoever bids highest takes the floor. A
  member whose urge never clears its threshold doesn't bid at all. That's the
  response-threshold model of division of labour in social insects, and it's
  what keeps a specialist quiet on questions it has nothing to add to.

### Why it converges instead of conforming

- **The first round is blind.** Each member deposits what it knows before it
  can read its peers, because a shared transcript destroys independence: the
  third speaker has already read the first two. And what a blind member is
  asked for is a *deposit*, not a position. On a desk of specialists, where one
  member holds the decisive fact and the rest share a prior, asking for
  positions lets the shared prior reach quorum before the informed member says
  anything. On the hidden-profile benchmark, asking for deposits takes a room
  from **16% to 67%** correct.
- **A reason to stop.** An episode ends on a quorum it can name, a deadlock
  between two carried options, a spent turn budget, or a room that has nothing
  to say. Never because one agent decided it was finished. One operator message
  is one bounded number of turns, whatever the desk's size.
- **Small budgets on purpose.** Conformity among language models rises with
  interaction time, so a long episode buys correlated error rather than better
  judgement. The default budget is small, and raising it is a decision you make
  per desk, not a setting you forget.
- **Cross-desk referral.** Members of one desk read the same transcript, work
  the same part of the company, and are wrong about the same things. Averaging
  correlated error doesn't remove it; only pooling *across* the boundary can. A
  desk can put a question to another desk, which answers with one real turn on
  its own channel, and what crosses carries information, never a vote. Three
  desks each confidently wrong about a different option: deliberating inside
  them scored **0.2%**; crossing between them, **77.5%**.
- **Private asides, off by default.** Two members of a desk can compare notes
  without the room, and the exchange is on the record even though the room
  can't read it. The library measured it: it loses 15 points on a hidden
  profile, because averaging inside one correlated desk imports the shared
  bias. So it's a knob you turn on deliberately, not a feature that's on.

### What you get out of it

Every episode closes with a line the operator can read: what carried, who
supported it, what was objected to and why, or that the desk deadlocked and
between what. Decisions that settled long ago stay on a pinboard so they don't
scroll away, and a desk remembers its past episodes. A desk of one behaves
byte-for-byte like a single agent, so nothing here costs you anything until a
desk has somebody to deliberate with.

[`docs/spec/runtime/hivemind.md`](docs/spec/runtime/hivemind.md) has the whole
mechanism, and the
[tinyhivemind benchmarks](https://github.com/tinyhumansai/tinyhivemind/wiki/Benchmarks)
have the numbers.

## What one person can now run

Every folder under [`companies/`](companies/) is a complete company you can
launch today, with a roster of agents, their responsibilities, and the handful
of moments where a human signs off:

| You want to run a… | The hive handles | You keep |
| --- | --- | --- |
| **[Venture Studio](companies/venture_studio/)** | Scouting, founding, building, launching, operating a portfolio | Capital allocation & strategy |
| **[Startup Accelerator](companies/startup_accelerator/)** | Sourcing, screening, mentoring, demo day, investor intros | Investment decisions |
| **[VC Firm](companies/venture_capital/)** | Deal flow, diligence, memos, portfolio support | The final "yes" |
| **[Consulting Firm](companies/consultation_firm/)** | Research, analysis, modeling, decks, implementation plans | Executive workshops |
| **[Software Company](companies/software_company/)** | PM, design, frontend, backend, QA, security, docs, support, DevRel | Product direction |
| **[Product Team](companies/product_team/)** | A triaged queue, a groomed backlog, a defended roadmap | Prioritization calls & roadmap sign-off |
| **[Marketing Agency](companies/marketing_agency/)** | Creative, copy, SEO, paid, email, landing pages, analytics | Campaign sign-off |
| **[Design Studio](companies/design_studio/)** | Branding, UI, motion, illustration, user testing | Creative direction |
| **[Media Company](companies/media_company/)** | Finding, verifying, writing, illustrating, distributing stories | Editorial standards |
| **[Influencer Brand](companies/influencer_business/)** | Scripting, editing, thumbnails, posting, community, sponsorships | Your face (or an avatar) |
| **[Game Studio](companies/game_studio/)** | Worlds, story, code, art, QA, balance, launch | Creative direction |
| **[Game Business](companies/game_business/)** | UA, monetization, LiveOps, community, store optimization | Growth strategy |
| **[Recruiting Firm](companies/recruiting_company/)** | Sourcing, outreach, screening, interviews, offers | Final hiring calls |
| **[Enterprise Sales](companies/enterprise_sales/)** | Lead gen, outreach, CRM, proposals, contracts, follow-up | Closing strategic accounts |
| **[Support Org](companies/customer_support/)** | Tickets, docs, bug reports, escalations, refunds | Policy & escalation |
| **[Real Estate Co](companies/realestate_company/)** | Sourcing, analysis, underwriting, contractors, tenants | Purchase approvals |
| **[Accounting Firm](companies/accounting_firm/)** | Bookkeeping, tax, payroll, forecasting, audit prep | Signing the filings |
| **[Law Firm](companies/law_firm/)** | Research, drafting, litigation support, discovery, compliance | Approving filings |
| **[Pharma Startup](companies/pharma_startup/)** | Literature, molecule discovery, simulation, trial planning | The lab work |
| **[Research Lab](companies/research_lab/)** | Source-backed research reports with the evidence attached | Setting the question & accepting findings |
| **[Math Lab](companies/math_lab/)** | Verified answers to computational problems, with the programs that produced them | Stating the problem & accepting the answer |
| **[Signals + Opportunity Studio](companies/signals_opportunity_studio/)** | Scouting signals, clustering pains, ranking opportunities into a weekly brief | Which opportunities to fund |

Twenty-two companies. One operator. Pick one and run it, or run several at once.
[`companies/README.md`](companies/README.md) has the full catalog.

## Quickstart

You do not need a software background to run a company. You need either
[Docker Desktop](https://www.docker.com/products/docker-desktop/) or Podman
with its Docker-compatible CLI and Compose provider, a terminal, and about
fifteen minutes. On Windows the terminal must be POSIX —
[WSL](https://learn.microsoft.com/windows/wsl/install) or Git Bash — because the
quickstart below uses `export` and `./scripts/launch-demo.sh`.

```sh
git clone --recurse-submodules https://github.com/tinyhumansai/opencompany.git
cd opencompany
export TINYHUMANS_API_KEY="th-..."          # grab yours at tinyhumans.ai
./scripts/init-demo-admin.sh marketing you@example.com
./scripts/launch-demo.sh marketing up
```

There is no bundled username or password. The initializer prompts for a
password without putting it in shell history and creates `you@example.com` as
the demo administrator. The first run takes a few minutes while it downloads
and builds. When it settles, open **<http://localhost:5173>** and sign in with
that email and password. That's the console, where you watch your agents work
and answer anything waiting on you.

Run the initializer once per demo data volume. Removing that volume with
`./scripts/launch-demo.sh marketing down -v` removes the account too, so run
the initializer again before the next launch.

`./scripts/list-demos.sh` lists the other businesses you can launch in place of
`marketing`, and `./scripts/launch-demo.sh marketing down` shuts it all down.

Prefer to build the host from source, deploy it somewhere, or change the runtime
itself? That path lives in [docs/running-locally.md](docs/running-locally.md):
Cargo builds, Compose, feature flags, the Tauri desktop preview, and
DigitalOcean / AWS deploys.

> **You'll want a TinyHumans API key.** It's what lets the agents think and
> act. Without one you can still launch a company and look
> around; the agents just won't do real work. Grab a key at
> **[tinyhumans.ai](https://tinyhumans.ai)** and
> `export TINYHUMANS_API_KEY="th-..."`.

## Why it works

- **A real org chart, not a prompt.** Each company is declared as a roster of
  agents with distinct mandates in a simple `company.toml`. The host
  instantiates them, coordinates them, and keeps them running.
- **Desks that think as a hive.** Any desk with two or more members answers as
  a room: an episode of bounded rounds that converges on a named option, with
  the proposals, support and objections on the record. A desk of one behaves
  exactly like a single agent. No fan-out, no vote-averaging.
- **Humans in the loop where it counts.** Every harness names the exact
  decisions reserved for you. Delegate the work; keep the judgment.
- **Built on proven runtimes.** OpenCompany is a light host over OpenHuman, the
  TinyHumans agent modules and tinyhivemind, so it reuses their runtime and
  their mechanics instead of reinventing them.
- **Rust-fast and inspectable.** An Axum HTTP surface, a small default build,
  and deeper capabilities behind feature flags. Simple to start, honest to
  operate, easy to test.
- **Yours to own.** GPL-3.0, self-hostable, no lock-in.
## Common Use Cases

TinyJuice can be useful in a variety of workflows:

- Compressing large terminal logs before sending them to an AI model
- Reducing token usage when analyzing code diffs
- Summarizing search results and web content
- Making long JSON outputs easier to inspect
- Keeping important errors and warnings visible in large logs
- Improving context efficiency for coding agents and automation tools
## Make it yours

Each company folder holds a `company.toml`, a plain text file naming the roles,
what each one owns, which desks they sit at, and where you want to be asked
before anything happens. It's written to be read by people; changing a role, or
tuning how a desk deliberates (its quorum, its turn budget, whether it can refer
a question to another desk), is editing a few lines rather than programming. `opencompany check` reports any problems in plain language, and
adding a new business is a new folder, not a new program.
[Your first company](docs/gitbooks/get-started/your-first-company.md) walks through it.

## What it reports about itself

Nothing, unless it is a tenant on the TinyHumans hosted platform.

- **A self-hosted or desktop install sends nothing** — and not "nothing by
  default" in the sense of a switch someone could flip. The network client is
  behind a cargo feature the shipped default build does not compile in, so
  there is no code in that binary that could make the request. Getting one out
  of that state takes a recompile, not a config change.
- **Hosted tenants report product usage**, because the platform builds their
  image with that feature on and injects a project token. What it reports is
  shape and outcome under an opaque id: how many companies are configured,
  which storage backend is in use, whether a turn finished or failed, and token
  and cost counts.
- **No company content ever leaves, on any install.** Not message text,
  prompts, agent output, file paths, ledger values, tool names or arguments,
  email addresses, company or agent names, task titles, error messages, or
  credentials of any kind. That is enforced by construction rather than by
  review: a reported property is a word compiled into the binary, a count, a
  number or a boolean, and the type has no `String` variant for runtime text to
  arrive in.
- **To turn it off**, set `OPENCOMPANY_ANALYTICS=off`. It outranks everything
  else, and boot prints one line saying which way it resolved.

[`docs/spec/runtime/analytics.md`](docs/spec/runtime/analytics.md) has every
event and property, the conditions that must all hold before anything is sent,
and how the opaque id is derived. Crash reporting is separate, off until you
configure it, and goes to your own Sentry project rather than ours —
[`docs/spec/runtime/crash-reporting.md`](docs/spec/runtime/crash-reporting.md).

## Documentation

| Where | What's there |
| --- | --- |
| [`docs/gitbooks/`](docs/gitbooks/README.md) | The full docs: what OpenCompany is, what one person can run, and how it holds together |
| [`docs/running-locally.md`](docs/running-locally.md) | Docker, Compose, from-source builds, feature flags, desktop preview, deploy targets |
| [`docs/repository-layout.md`](docs/repository-layout.md) | Where everything lives in the tree and what each package owns |
| [`docs/spec/README.md`](docs/spec/README.md) | Architecture reference |
| [`docs/spec/runtime/hivemind.md`](docs/spec/runtime/hivemind.md) | How a desk deliberates: episodes, rounds, quorum, [referral](docs/spec/runtime/hivemind-referral.md) and [asides](docs/spec/runtime/hivemind-asides.md) |
| [`docs/gitbooks/developers/`](docs/gitbooks/developers/README.md) | Build, CLI, authoring companies, deployment, configuration |
| [`scripts/qa/`](scripts/qa/README.md) | Checking a release against a deployed tenant |

## Contributing

New here? Start with the
[good first issues](https://github.com/tinyhumansai/opencompany/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22),
which are scoped to be finishable in a sitting.
[CONTRIBUTING.md](CONTRIBUTING.md) has the local checks to run before opening a
pull request, and anything big enough to break an existing company starts as an
[RFC](https://github.com/tinyhumansai/opencompany/discussions/categories/rfcs)
rather than a PR.

## Community

[Discussions](https://github.com/tinyhumansai/opencompany/discussions) is where
questions get answered and large changes get argued out before they're built.
[docs/SUPPORT.md](docs/SUPPORT.md) says which channel takes what.

- **Discord**: <https://discord.tinyhumans.ai>
- **X**: [@tinyhumansai](https://x.com/tinyhumansai)
- **Reddit**: [r/tinyhumansai](https://www.reddit.com/r/tinyhumansai/)
- **Website**: [tinyhumans.ai/opencompany](https://tinyhumans.ai/opencompany)

## Star us on GitHub

_Running a company with a headcount of one? Star the repo and help others find the path._

<p align="center">
 <a href="https://www.star-history.com/#tinyhumansai/opencompany&type=date&legend=top-left">
 <picture>
 <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=tinyhumansai/opencompany&type=date&theme=dark&legend=top-left" />
 <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=tinyhumansai/opencompany&type=date&legend=top-left" />
 <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=tinyhumansai/opencompany&type=date&legend=top-left" />
 </picture>
 </a>
</p>

## Contributors Hall of Fame

Show some love and end up in the hall of fame. Contributors get free merch and special access to our [Discord](https://discord.tinyhumans.ai/).

<a href="https://github.com/tinyhumansai/opencompany/graphs/contributors">
 <img src="https://contrib.rocks/image?repo=tinyhumansai/opencompany" alt="OpenCompany contributors" />
</a>

## License

OpenCompany is licensed under the GNU General Public License v3. See
[LICENSE](LICENSE).
