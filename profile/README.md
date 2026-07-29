<div align="center">

# LemeBreak

### Describe a security weakness in plain English. Get a real, exploitable lab you can break into - with an AI tutor that teaches you how.

**[lemebreak.ai](https://lemebreak.ai)**

</div>

---

## What it is

LemeBreak turns a natural-language prompt into a fully functional, intentionally vulnerable environment, deploys it, and teaches you to exploit it.

You describe the scenario - *"an SSRF that reaches a cloud metadata endpoint and steals a managed-identity token"* - and an AI agent writes the vulnerable system, stands it up in an isolated ephemeral environment, verifies the exploit actually works, and drops you into a browser workspace with an attacker shell, the target, and a tutor grounded in the real source.

No slides. No pre-canned CTF. A live thing you break with your own hands.

## See it in action

https://github.com/user-attachments/assets/0bb595a7-6fdb-4820-86be-954eaf4e5f08

## What makes it different

**Real vulnerabilities, real evidence.**
There are no `FLAG{}` strings, no planted victory files, no fake "you win" endpoints.
Success is measured the way it is in the real world: data you exfiltrated, a shell you landed, a secret you stole, an admin role you escalated into.

**The lab is generated, not retrieved.**
Every environment is built on demand by an agent that writes the code, builds it, runs it, and confirms the exploit path end to end before you ever see it - so what you attack is genuinely vulnerable, not a mockup.

**A tutor that actually knows this lab.**
The embedded AI tutor is grounded in the specific environment you're attacking - its real source, its real topology - so the guidance is about *your* target, not generic advice.

**Ephemeral by design.**
Every lab is isolated and time-boxed, then torn down cleanly. Nothing lingers.

## Two kinds of labs

LemeBreak generates across two substrates, chosen automatically from what you describe.

| | **App labs** | **Cloud labs** |
|---|---|---|
| **You learn** | Web & application exploitation | Cloud-identity offense |
| **The target** | A vulnerable containerized app | An ephemeral cloud identity environment |
| **Examples** | SQL injection, SSRF, command injection, insecure deserialization, auth bypass, IDOR chains | Managed-identity privilege escalation, SSRF-to-secrets, tenant takeover |
| **Evidence of success** | Stolen data, a shell, leaked secrets | A stolen token, a read secret, a Global Admin assignment |

## How it works

```
  "Build me a lab where an SSRF leaks cloud credentials"
                        │
                        ▼
        ┌──────────────────────────────┐
        │  Conversation                │   The agent clarifies what you
        │                              │   want and proposes a build plan.
        └──────────────┬───────────────┘
                        ▼
        ┌──────────────────────────────┐
        │  Agentic build & verify      │   It writes the environment, builds
        │                              │   it, and proves the exploit works -
        │                              │   fixing and retrying until it does.
        └──────────────┬───────────────┘
                        ▼
        ┌──────────────────────────────┐
        │  Deployment                  │   The verified lab is provisioned
        │                              │   into an isolated, ephemeral
        │                              │   environment of its own.
        └──────────────┬───────────────┘
                        ▼
        ┌──────────────────────────────┐
        │  Workspace                   │   A browser IDE: attacker shell,
        │                              │   the live target, a situational
        │                              │   map, and an AI tutor + guide.
        └──────────────────────────────┘
```

## Architecture at a glance

LemeBreak is a browser-first platform backed by an agent that builds and drives real infrastructure.

```
   Browser (React portal)
        │
        ▼
   Agent Gateway  ──────▶  Orchestrator  ──────▶  App labs   (isolated containers)
   - conversation                                    │
   - agentic build loop                              └────▶  Cloud labs (ephemeral cloud
   - live workspace relay                                    identity environments,
   - grounded AI tutor                                       provisioned as code)
```

- **Portal** - a single-page browser workspace: generate a lab, then attack it from a live three-panel IDE (shell, target, tutor + guide) with a situational-awareness map for cloud labs.
- **Agent Gateway** - the brain: it runs the conversational front door, the agentic build-and-verify loop, and the grounded tutor, and relays the live workspace.
- **Orchestrator + lab substrates** - provision each verified lab into its own isolated, time-boxed environment, and guarantee a clean teardown.

## Built with

| Layer | Technology |
|---|---|
| Agents & tutor | Claude (Anthropic API) |
| Backend | Python, FastAPI, server-sent events |
| Frontend | React 19, TypeScript, Tailwind CSS, Vite |
| App-lab substrate | Docker, reverse-proxy routing |
| Cloud-lab substrate | Infrastructure-as-code on Azure / Entra |
| Platform | Cloudflare, Supabase |

## Safety & isolation

LemeBreak generates deliberately vulnerable environments, so containment is a first-class concern.

- Every lab is **isolated** and cannot reach other labs or the platform.
- Every lab is **ephemeral** and torn down automatically, on every substrate.
- Cloud labs run inside a **policy cage** with a guaranteed teardown, so nothing outlives its session.
- Access to each lab is **scoped and authenticated** per session.

---

<div align="center">

### Ready to break something?

**[→ Try it at lemebreak.ai](https://lemebreak.ai)**

</div>
