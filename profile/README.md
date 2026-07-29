<div align="center">

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

https://github.com/user-attachments/assets/d182ac9c-4126-4b02-a5cd-ad02e8e7c275

## Highlights

- **Prompt to a real lab.** Describe a CVE, a misconfiguration, or an attack scenario in plain English; an agent writes the vulnerable system, builds it, and confirms the exploit end to end before you touch it.
- **Two substrates.** Containerized web-application labs, and ephemeral cloud-identity labs on Azure / Entra - chosen automatically from what you describe.
- **Real evidence, not flags.** No `FLAG{}`, no planted files, no simulations. Success is exfiltrated data, a landed shell, a stolen secret, an escalated role.
- **A workspace, not a walkthrough.** A browser IDE with an attacker shell, the live target, and - for cloud labs - a situational-awareness graph of identities and trust relationships.
- **An AI research companion.** A seasoned offensive-security practitioner grounded in the live environment, where the conversation and the lab are the same surface.
- **Isolated and ephemeral.** Every lab is network-isolated, TTL-enforced, and torn down automatically.

## How it works

| Step | |
|---|---|
| **Describe** | A CVE, a misconfiguration, or an attack scenario - in plain English. |
| **Build** | An agent writes the vulnerable environment, builds it, and proves the exploit works, fixing and retrying until it does. |
| **Break** | You get a live workspace and exploit a real target. Success is real evidence, not a flag. |

## The research companion

The exploit guide isn't a static document - it's a colleague.

LemeBreak's companion is a seasoned offensive-security practitioner grounded in *your* lab: its real source, its real topology, and the live state of the environment you're attacking.
Because the conversation and the environment are the same surface, an idea becomes a live action with no friction - you talk through a hunch and it spins up the resource, runs the command, and lays out what came back.
That is what makes it an IDE and not a chat window.

A sample exchange:

> **You:** what's this lab?
>
> **Companion:** classic SSRF-to-metadata setup - the vault's the prize. Want to poke it together, or the mental model first?
>
> **You:** let's pull the token
>
> **Companion:** got it. ...that worked. The more interesting move now isn't the secret - it's that this same identity can probably reach the storage account. Want to chase that?

It answers the question, then points at the door you didn't see.

## Lab types

| | App labs | Cloud labs |
|---|---|---|
| Focus | Web & application exploitation | Cloud-identity offense |
| Target | A vulnerable containerized app | An ephemeral cloud identity environment |
| Examples | SQL injection, SSRF, command injection, insecure deserialization, auth bypass, IDOR chains | Managed-identity privilege escalation, SSRF-to-secrets, tenant takeover |
| Evidence | Stolen data, a shell, leaked secrets | A stolen token, a read secret, a Global Admin assignment |

## The workspace

Once a lab is live, you attack it from a browser IDE:

- **Attacker shell** - a real terminal on the same network as the target.
- **Live target** - the vulnerable app or cloud environment.
- **Situational-awareness graph** - for cloud labs, a live map of identities, resources, and the trust relationships between them.
- **Companion + guide** - the research partner, plus a walkthrough generated from the lab's actual internals.

## Architecture

A browser-first platform backed by an agent that builds and drives real infrastructure.

```
   Browser IDE (React)
   companion · shell · target · map
        │
        ▼
   Agent Gateway  ──────▶  Orchestrator  ──────▶  App labs   (isolated containers)
   - co-design conversation                          │
   - agentic build & verify loop                     └────▶  Cloud labs (ephemeral cloud
   - the grounded companion                                  identity environments,
   - live workspace relay                                    provisioned as code)
```

- **Browser IDE** - generate a lab through conversation, then attack it from a live workspace.
- **Agent Gateway** - the co-design front door, the agentic build-and-verify loop, and the environment-grounded companion.
- **Orchestrator + substrates** - provision each verified lab into its own isolated, time-boxed environment, with a guaranteed teardown.

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
- Access to each lab is **scoped and authenticated** per session.

## Links

- Website: **[lemebreak.ai](https://lemebreak.ai)**
- Status: beta
