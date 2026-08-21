<div align="center">

<img src="https://raw.githubusercontent.com/AI-security-labs/.github/main/profile/lb-logo.svg" width="76" alt="LemeBreak" />

# LemeBreak

**Real, exploitable security labs on demand - with an AI research companion.**

[![Website](https://img.shields.io/badge/lemebreak.ai-6366F1?style=flat-square&logo=safari&logoColor=white)](https://lemebreak.ai)
&nbsp;![Status](https://img.shields.io/badge/status-beta-9333EA?style=flat-square)
&nbsp;![React](https://img.shields.io/badge/React-19-0284C7?style=flat-square&logo=react&logoColor=white)
&nbsp;![FastAPI](https://img.shields.io/badge/FastAPI-Python-009688?style=flat-square&logo=fastapi&logoColor=white)

</div>

LemeBreak turns a natural-language prompt into a genuinely vulnerable, ephemeral environment, verifies the exploit works, and drops you into a browser workspace to break it - alongside an AI companion grounded in that exact lab.

It's hosted at **[lemebreak.ai](https://lemebreak.ai)** and currently in beta. This page is a project overview.

## Demo

A single prompt becomes a live Azure lab: the builder agent designs and stands up the vulnerable environment, then a blind red team AI operator attacks it on its own - SSRF to instance metadata, managed-identity theft, an ARM `listKeys` bypass, and private blob exfiltration.

https://github.com/user-attachments/assets/28f7f432-1529-4df4-bab3-4fd49ea8ef16

## Highlights

- **Prompt to a real lab.** Describe a CVE, a misconfiguration, or an attack scenario in plain English; an agent writes the vulnerable system, builds it, and confirms the exploit end to end before you touch it.
- **Two substrates, one pipeline.** Containerized web-application labs, and ephemeral cloud-identity labs on Azure / Entra - chosen automatically from what you describe, and built by the same machinery.
- **Real evidence.** Success is exfiltrated data, a landed shell, a stolen secret, an escalated role. No `FLAG{}`, no planted files, no simulations.
- **A browser IDE.** An attacker shell, the live target, and - for cloud labs - a situational-awareness graph of identities and trust relationships.
- **An AI research companion.** A seasoned offensive-security practitioner grounded in the live environment, where the conversation and the lab are the same surface.
- **A blind red team AI operator.** Hand a lab to an autonomous operator that gets no walkthrough and no answer key, then watch the attack chain build in real time and take the wheel whenever you want.
- **Isolated and ephemeral.** Every lab is network-isolated, TTL-enforced, and torn down automatically.

## How it works

| Step | |
|---|---|
| **Describe** | A CVE, a misconfiguration, or an attack scenario - in plain English. |
| **Build** | An agent writes the environment, exploits it to prove it works, and hands off an artifact. |
| **Break** | You get a live workspace and a real target. Success is real evidence. |

## One pipeline, two substrates

A container lab and a cloud-identity lab have almost nothing in common at the bottom.
One is a set of images on a private network; the other is an ephemeral cloud tenancy with real identities and real access policies.

They are the same product anyway, because they run the same pipeline.
The same co-design conversation, the same build loop, the same handoff, the same provisioning call, and the same workspace.
The substrate decides what the agent writes, which extra tooling it can reach, how the lab is described to the orchestrator, and how teardown works.

**The seam that makes that possible is a small shared contract.**
Both substrates are driven through the same four verbs - launch, stop, extend, status - and answer in JSON.
So the orchestrator treats a container runtime and a cloud tenancy identically.
It launches a lab, asks how long it has left, and tears it down, and the substrate underneath stays an implementation detail.

Everything above that line is written once.

## Inside a build

The interesting part of LemeBreak is what happens between those three steps.
A model can write a plausible vulnerable app in one shot.
Getting an environment that is actually exploitable, actually reachable, and accurately described is a different problem, and most of the platform exists to solve it.

**1. Co-design, before anything is built.**
The first conversation is with the companion.
You can talk through an idea, ask how a technique works, or chase a tangent, and nothing is provisioned.
During this phase the agent's toolset physically excludes the build tools, so a conversation cannot quietly turn into a deployment.
When you land on something concrete the companion proposes it, picks the substrate that fits, and waits for you to approve.

**2. The build loop.**
On approval, a builder agent takes over with a working directory and a small set of tools: write files, read files, edit, run whitelisted commands, look something up on the web, and one terminal "done" call.
It is seeded with a vetted application starter so it begins from a known-good rendering core.
It writes the environment, builds it, runs it, and attacks it, iterating against real output.

**3. It has to break in itself.**
The agent is required to exploit its own lab and come back with real evidence before it can hand anything off.
Evidence means exfiltrated data, a landed shell, a recovered secret, or an escalated role.
The full transcript of that attempt is kept.

When it calls "done", the platform stops taking its word for anything.

## What gets checked before a lab ships

The model writes both the lab and the description of the lab, so the platform verifies the description independently.
These checks run automatically at handoff, before the lab is presented to anyone.
A failure is handed back to the agent to fix, twice, before the build fails honestly.

| Check | What it does |
|---|---|
| **Render check** | The built application is started in a throwaway container and crawled from its front page. Unrendered template placeholders, an empty page body, or a server error fail the build. It reads rendered output, so it is stack-agnostic. |
| **Attack-path binding** | On cloud labs, wherever the agent binds a step of the attack path to a real resource, that resource must exist in the lab's own infrastructure code. A step claiming a vault the lab never created is rejected outright. |
| **Template detection** | A declared attack path that reuses the shape of the reference example for an unrelated vulnerability is flagged. Plausible-sounding structure with nothing behind it is exactly the failure mode this catches. |
| **Static validation** | Cloud labs are validated before a single metered resource is created, so a malformed environment fails for free in seconds. |
| **Liveness** | Every lab must come up healthy on its substrate before it is handed over, and cloud labs are re-probed at their public entry point after deployment. |

## Build once, deploy fresh

The agent builds and verifies inside a throwaway sandbox, and that sandbox is then destroyed.
What survives is the artifact: a machine-readable blueprint for a container lab, or the infrastructure code for a cloud lab.
Your instance is deployed fresh from that artifact.

This separation is deliberate, and it is what makes a lab a stable, addressable thing.

- The same prompt twice gives you two different labs, by design. Variety is a feature of a generator.
- The same **lab** is reproducible. Relaunching one replays its saved artifact, and that rebuild path is pure replay.
- The environment the agent verified and the environment you receive are produced from the same source.

## What a lab knows about itself

A lab is a running environment plus the record that environment produced.

Alongside the artifact it was built from, every lab keeps an inventory of its services and reachable entry points, its attack path, a walkthrough generated from its actual internals, the complete build transcript, any credentials the lab deliberately hands the learner, and a record of every engagement ever run against it. Cloud labs add the full resource and identity map described below.

That single record is why several features that look unrelated are actually the same thing:

- The companion is grounded in it, which is how it can talk about *your* lab specifically.
- The guide is generated from it, from the lab's actual internals.
- A red team engagement is graded against it, because it contains the attack path the lab was built around.
- Relaunching replays it, and a finished engagement can be reopened and replayed from it days later.

Build one good record per lab and the rest stops being separate work.

## The lab graph

Cloud labs come with a live map of the environment: the resources, the identities, and the trust relationships between them.

The map is **generated from the environment itself.**
LemeBreak built the environment, so it already owns ground truth that an attacker tool can only partially reconstruct from a stolen token.
The graph is built from the lab's real infrastructure state and live inventory, and edges are drawn from actual resource relationships.

The agent's role is narrow on purpose.
It declares the shape of the attack path, which resources sit on it, and in what order.
What each node *is* comes from the environment itself.
So everything on the map corresponds to something the lab actually contains, and two labs built months apart describe the same kind of resource the same way.

The graph also reconciles against live state while you work, so a resource you or the companion stand up mid-session appears on it.

## The research companion

The exploit guide is a colleague.

LemeBreak's companion is a seasoned offensive-security practitioner grounded in *your* lab: its real source, its real topology, and the live state of the environment you're attacking.
Because the conversation and the environment are the same surface, an idea becomes a live action with no friction - you talk through a hunch and it spins up the resource, runs the command, and lays out what came back.
That is what makes it an IDE.

A sample exchange:

> **You:** what's this lab?
>
> **Companion:** classic SSRF-to-metadata setup - the vault's the prize. Want to poke it together, or the mental model first?
>
> **You:** let's pull the token
>
> **Companion:** got it. ...that worked. Here's the interesting part: that same identity can probably reach the storage account. Want to chase that?

It answers the question, then points at the door you didn't see.

**Grounded means grounded.**
Before the companion states a runtime fact - what an environment variable holds, what the shell is authenticated as, what a resource is really named - it reads it from the live environment or runs a command to find out.
It draws a hard line between design facts, which it legitimately knows from having helped build the lab, and runtime values, which it has to confirm.
Build-time values are treated as wrong for your running lab, because they came from a sandbox that no longer exists.

## The red team operator

Cloud and app labs both come with a second agent. You watch this one work.

The operator starts on a foothold inside your lab knowing exactly one thing: the target's front door. No walkthrough, no source, no map of the environment, no hint about where the weakness is. Everything it knows, it earns by running a command and reading what comes back.

It has two tools. Run one shell command in the foothold, or file a report. That's the whole interface. What to look at, what to try, what to chase, and whether it has actually landed something are all its own calls, made with a full offensive kit against real infrastructure.

You watch the chain build in real time, one step at a time, with its reasoning in the open. Leave it alone and you get a clean run, front door to whatever it walks out with. Or steer it mid-engagement, point it somewhere new, and take the wheel whenever you want.

It works from its own isolated, throwaway container, and it's capped on both steps and wall-clock time.

### Keeping it blind

Blindness is an engineering problem, and most of the work happens in the environment.
An agent that can read its way to a head start tests nothing.

- **The context carries no answer key.** The operator's prompt is assembled separately from the companion's. The lab guide, the attack path, the build transcript, and the vulnerability declaration are all absent by construction.
- **The image is a clean room.** It runs on a build of the attacker toolkit with LemeBreak's own platform files stripped out, so there is nothing on disk to orient itself with.
- **Naming is neutralised.** Labs are barred from labelling themselves in metadata the environment will hand back on request, and container and network naming is scrubbed for the same reason, because a name that identifies the lab is a spoiler delivered by the first reverse lookup.
- **The target is an address.** The operator is told where the front door is, the way any real operator would be. How it is built, what is behind it, and where it is weak are all its to work out.

Getting this right took several passes, and each leak was found by reading what the operator actually did.

### Judging an engagement

Because LemeBreak built the target, it can grade the attempt.

Every finished engagement is scored against the lab's own attack path.
The grading rule that matters: credit requires **outcome evidence in command output**.
A stage that needed a token requires a token that actually came back.
A stage counts when the result was obtained, so an attempt, a mention, or an intention leaves it uncredited, and an authorization failure means the stage was not reached.

Coverage is reported alongside how far the operator got, how many moves it spent getting there, and any meaningful work it did outside the intended path.
Runs are also classified by how they ended, so a run that was cut short is reported separately from one that ran to its own conclusion.

## Lab types

Two shapes, same pipeline.

| | App labs | Cloud labs |
|---|---|---|
| Focus | Web & application exploitation | Cloud-identity offense |
| Target | A vulnerable containerized app | An ephemeral cloud identity environment |
| Examples | SQL injection, SSRF, command injection, insecure deserialization, auth bypass, IDOR chains | Managed-identity privilege escalation, SSRF-to-secrets, tenant takeover |
| Evidence | Stolen data, a shell, leaked secrets | A stolen token, a read secret, a Global Admin assignment |

## The workspace

Once a lab is live, you attack it from a browser IDE built as a set of applications.
One canvas, one application at a time, everything reachable in a keystroke.

- **Attacker shell** - a real terminal on the same network as the target, with as many sessions as you want.
- **Live target** - the vulnerable app or cloud environment.
- **Lab graph** - for cloud labs, the live map of identities, resources, and the trust relationships between them.
- **Observe / Identity / HTTP** - for cloud labs, what the control plane recorded, who your shell currently is, and the API traffic your session generated.
- **Attack surface** - the reachable entry points, and which of them take input.
- **Operator** - the blind engagement, live or replayed.
- **Companion + guide** - the research partner, plus a walkthrough generated from the lab's actual internals.

Panels stay alive when you switch between them, so a running shell and a live engagement both survive navigation.

## Architecture

A browser-first platform backed by an agent that builds and drives real infrastructure.

```
   Browser IDE (React)
   companion · shell · target · graph · operator
        │  server-sent events
        ▼
   Agent Gateway
   co-design · build loop · pre-flight gates · companion · operator · workspace relay
        │
        ├──▶  Build sandbox          throwaway: the agent writes, builds, and
        │                            exploits here, then it is destroyed
        │
        └──▶  Orchestrator  ────┬──▶  App labs     isolated containers
              launch · stop     │
              extend · status   └──▶  Cloud labs   ephemeral identity environments,
                                                   provisioned as code
```

- **Browser IDE.** Generate a lab through conversation, then attack it from a live workspace.
- **Agent Gateway.** The co-design front door, the build-and-verify loop, the gates, the companion, and the operator. Everything the models touch runs through here.
- **Orchestrator.** Validates each verified artifact, provisions it onto the right substrate, and owns the lifecycle: time limits, teardown, and a reconciliation sweep.
- **Substrates.** Containers for app labs, infrastructure-as-code for cloud labs, behind the one contract described above.

**The gateway is also the lab's front door.**
Every lab is reached through the gateway, on the platform's own origin: its web surface, its terminals, and its live data all arrive that way.
That is what makes a lab embeddable in the workspace at all, and it is the reason your session credentials can be stripped on the way in: the platform holds them, and the lab never sees them.

**Builds are detached from the browser.**
A cloud build can run for a long time, so the build runs independently of your connection.
It publishes into a replayable stream that you subscribe to.
The build survives a closed tab, and reopening rejoins exactly where you left off, terminal events included.
Cancelling is an explicit request.

**Three agent loops, one vocabulary.**
The builder, the companion, and the operator are very different agents doing very different jobs, but they all report in the same shape: a step, what the model said, the command it ran, and what came back.
So one renderer draws all three, and a finished engagement replays from its stored record in that same vocabulary.
Watching an agent work and reading back what it did are the same view.

## Tech stack

| Layer | Technology |
|---|---|
| Companion & build agent | Claude (Anthropic API) |
| Backend | Python, FastAPI, server-sent events |
| Frontend | React 19, TypeScript, Tailwind CSS, Vite |
| App-lab substrate | Docker, reverse-proxy routing |
| Cloud-lab substrate | Infrastructure-as-code on Azure / Entra |
| Platform | Cloudflare, Supabase |

## Safety & isolation

LemeBreak generates deliberately vulnerable environments, so containment is a first-class concern.

- Every lab is **isolated** and cannot reach other labs or the platform.
- Every lab is **ephemeral** and **TTL-enforced**, torn down automatically on every substrate.
- Cloud labs run inside a **policy cage** with a guaranteed teardown.
- Access to each lab is **scoped and authenticated** per session, and session credentials are stripped from anything forwarded into a lab, because a lab that reflects what it is sent should have nothing worth reflecting.
- The build agent runs against a **restricted command set** in an environment stripped of platform credentials, and outbound lookups are guarded against reaching internal addresses.
- Concurrent builds are **scoped to their own run** at the command layer, so one build's cleanup can never reach another's.

**Teardown is verified.**
Cloud labs are torn down across both planes, infrastructure and directory, because deleting a resource group leaves identity objects that outlive it.
Teardown then asserts that both planes are actually empty and reports honestly when they are not.

**And a sweep that trusts none of the above.**
Synchronous teardown only runs when something calls it, so a separate reconciliation pass independently cross-references live infrastructure against the platform's own records and reclaims anything that should be gone: expired labs, orphans from a crashed build, and leftovers from a teardown that half-failed.
It runs on a timer, on both substrates.

## Links

- Website: **[lemebreak.ai](https://lemebreak.ai)**
- Status: beta
