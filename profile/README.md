<div align="center">

# LemeBreak

### Describe a security scenario in plain English. Get a real, exploitable lab - then break it alongside an AI companion that researches with you.

**[lemebreak.ai](https://lemebreak.ai)**

</div>

---

## See it in action

https://github.com/user-attachments/assets/d182ac9c-4126-4b02-a5cd-ad02e8e7c275

---

## What it is

LemeBreak turns a natural-language prompt into a genuinely vulnerable environment, deploys it, and drops you into a browser workspace to break it - with an AI companion working the problem beside you.

You describe the scenario - *"an SSRF that reaches a cloud metadata endpoint and steals a managed-identity token"* - and an agent writes the vulnerable system, stands it up in an isolated, ephemeral environment, and verifies the exploit path actually works before you ever touch it.

Then the real product begins: a live workspace with an attacker shell, the target, a map of the terrain, and a companion who knows this exact environment and helps you tear into it.

No slides. No pre-canned CTF. **No simulations.** A real target you break with your own hands.

## Describe → Build → Break

| | |
|---|---|
| **Describe** | A CVE, a misconfiguration, or an attack scenario - in plain English. |
| **Build** | An agent writes the vulnerable environment, builds it, and proves the exploit works - fixing and retrying until it does. |
| **Break** | You get a live workspace and exploit a real target. Success is real evidence, not a flag. |

## Not a chatbot. An IDE with a companion.

The exploit walkthrough isn't a static document - it's a colleague.

LemeBreak's companion is a seasoned offensive-security practitioner who's grounded in *your* lab: its real source, its real topology, the live state of the environment you're attacking. It doesn't hand you generic advice. It sees where things connect, opens the next door, and - when you want it - gets its hands dirty with you.

Because the conversation and the environment are the **same surface**, an idea becomes a live thing with no friction. You talk through a hunch, and it spins up the resource, runs the command, and lays out what came back - all in one continuous motion. That's what makes it an IDE and not a chat window.

> *"This lab's a classic SSRF-to-metadata setup - the vault's the prize. Wanna poke it together, or you want the mental model first?"*
>
> *"Nice, that worked. The more interesting move now isn't the secret - it's that this same identity can probably reach the storage account. Want to chase that?"*

It answers your question - and hands you the next thread to pull.

## Real vulnerabilities, real evidence

There are no `FLAG{}` strings, no planted victory files, no fake "you win" endpoints.

Success is measured the way it is in the real world: data you exfiltrated, a shell you landed, a secret you stole, an admin role you escalated into. Every environment is built on demand by an agent that writes the code, runs it, and confirms the exploit path end to end - so what you attack is genuinely vulnerable, not a mockup.

## Two kinds of labs

LemeBreak generates across two substrates, chosen automatically from what you describe.

| | **App labs** | **Cloud labs** |
|---|---|---|
| **You learn** | Web & application exploitation | Cloud-identity offense |
| **The target** | A vulnerable containerized app | An ephemeral cloud identity environment |
| **Examples** | SQL injection, SSRF, command injection, insecure deserialization, auth bypass, IDOR chains | Managed-identity privilege escalation, SSRF-to-secrets, tenant takeover |
| **Evidence of success** | Stolen data, a shell, leaked secrets | A stolen token, a read secret, a Global Admin assignment |

## Inside the workspace

Once a lab is live, you attack it from a browser IDE:

- **Attacker shell** - a real terminal on the same network as the target, ready to go.
- **The live target** - the vulnerable app or cloud environment, right there.
- **Situational-awareness map** - for cloud labs, a live graph of identities, resources, and the trust relationships between them, so you can see the attack surface instead of guessing at it.
- **The companion + guide** - the research partner, plus a walkthrough generated from the lab's actual internals.

## Architecture at a glance

LemeBreak is a browser-first platform backed by an agent that builds and drives real infrastructure.

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

- **Browser IDE** - generate a lab through conversation, then attack it from a live workspace: shell, target, situational-awareness map, and the companion.
- **Agent Gateway** - the brain: the co-design front door, the agentic build-and-verify loop, and the environment-grounded companion.
- **Orchestrator + lab substrates** - provision each verified lab into its own isolated, time-boxed environment, and guarantee a clean teardown.

## Built with

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
- Every lab is **ephemeral** and **TTL-enforced** - torn down automatically, on every substrate.
- Cloud labs run inside a **policy cage** with a guaranteed teardown, so nothing outlives its session.
- Access to each lab is **scoped and authenticated** per session.

---

<div align="center">

### Ready to break something?

**[→ Try it at lemebreak.ai](https://lemebreak.ai)**

</div>
