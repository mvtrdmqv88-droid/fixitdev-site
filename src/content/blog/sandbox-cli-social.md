# Sandbox CLI -- Social Media Drafts

## Twitter/X

### Post 1: Introduction
I kept polluting my laptop with tools I only needed once. So I built Sandbox CLI -- run `sandbox ssh`, get a fresh Ubuntu container, disconnect and it auto-destroys. Free + Pro ($4.99/mo). https://mvtrdmqv88-droid.github.io/fixitdev-site/tools/sandbox-cli/ #devtools #docker #cli

### Post 2: Feature Demo
`sandbox ssh` -- that's the whole workflow.

Fresh Ubuntu container. Real SSH session. Auto-destroys on disconnect.

No Dockerfiles. No docker-compose. No cleanup.

Built with Go + Docker SDK. Dashboard included.

https://mvtrdmqv88-droid.github.io/fixitdev-site/tools/sandbox-cli/

### Post 3: Engagement
What's the weirdest thing you've found installed on your dev machine that you have zero memory of installing?

I found three different Java JDKs and a MongoDB that had been running for 8 months.

That's why I started using disposable containers for everything: https://mvtrdmqv88-droid.github.io/fixitdev-site/tools/sandbox-cli/

---

## Reddit

### r/SideProject

**Title:** I built a CLI that gives you instant throwaway dev environments via SSH

I kept running into the same problem: I'd install something to try it out, forget about it, and six months later my laptop was a mess of conflicting tools and mystery background services.

So I built Sandbox CLI. You run `sandbox ssh`, it spins up a fresh Ubuntu container, gives you SSH access, and when you disconnect, the container auto-destroys. No Dockerfiles, no cleanup, no leftover junk.

Tech stack: Go for the CLI/daemon, Docker SDK for container management, SQLite for session tracking, and a Next.js dashboard for a visual overview.

Free tier: 3 containers/day, 30-minute sessions. Pro: $4.99/month for unlimited everything.

Would love feedback. What would make this more useful for your workflow?

GitHub: https://github.com/mvtrdmqv88-droid/sandbox-cli
Site: https://mvtrdmqv88-droid.github.io/fixitdev-site/tools/sandbox-cli/

### r/devops (or r/programming)

**Title:** Sandbox CLI: instant ephemeral Ubuntu containers via SSH, auto-cleanup on disconnect

Open source CLI tool (Go + Docker SDK) that manages throwaway dev containers.

**What it does:**
- `sandbox ssh` creates a fresh Ubuntu container and drops you into an SSH session
- On disconnect, the container is automatically destroyed
- Includes a Next.js web dashboard for session monitoring
- Session data stored in local SQLite

**Why SSH instead of docker exec:**
- Proper TTY handling, works with all your existing SSH config/tools
- Port forwarding works naturally
- Feels like a real remote machine, not a container hack

**Use cases:**
- Testing something without polluting your host machine
- Reproducing bugs in a clean environment
- Quick experiments with new tools/languages
- Onboarding (no more "setup guide" docs)

Free tier: 3 containers/day, 30-min limit. Pro ($4.99/mo): unlimited.

https://github.com/mvtrdmqv88-droid/sandbox-cli

### r/docker (niche)

**Title:** Built a tool to automate the "spin up container, SSH in, destroy when done" workflow

I was doing this manually all the time -- create a container, set up SSH, do my thing, remember to clean up. Automated the whole thing into a single CLI command.

`sandbox ssh` -- creates container, configures SSH, connects you. `exit` -- container is gone.

Uses Docker SDK under the hood so it's native Docker, not some wrapper layer. Also tracks sessions in SQLite and has a dashboard.

Anyone else have this workflow? Curious what images/configs you'd want supported.

https://github.com/mvtrdmqv88-droid/sandbox-cli

---

## Product Hunt

**Tagline:** Instant throwaway dev environments. One command. Auto-cleanup.

**Description:**
Sandbox CLI lets developers spin up fresh Ubuntu containers via SSH with a single command. No Dockerfiles, no configuration -- just `sandbox ssh` and you're in a clean environment. When you disconnect, the container self-destructs.

Built for developers who are tired of polluting their local machines with tools they only need temporarily. Includes a web dashboard for monitoring active sessions and usage history.

Free: 3 containers/day, 30-min sessions.
Pro ($4.99/mo): Unlimited containers, no time limits.

**Maker Comment:**
Hey everyone! I'm the developer behind Sandbox CLI. I built this because I kept trashing my laptop with random installations for tutorials and experiments. The "install, try, forget to uninstall" cycle was getting out of hand.

The core idea is dead simple: make it trivially easy to get a clean Linux environment and equally easy to throw it away. No Dockerfiles, no docker-compose, no cleanup scripts. Just `sandbox ssh` and `exit`.

The tech stack is Go for the CLI and daemon (fast startup, low memory), Docker SDK for container management (no shelling out to docker CLI), SQLite for local session data, and Next.js for the dashboard.

I'd love to hear how you'd use this and what features would make it more useful for you. All feedback welcome!

---

## Hacker News

**Title:** Show HN: Sandbox CLI -- Instant Ephemeral Dev Environments via SSH

**Text:**
I built a CLI tool that automates the "spin up a Docker container, SSH into it, destroy it when done" workflow.

Run `sandbox ssh` and you get a fresh Ubuntu container with SSH access. When you disconnect, the container is automatically cleaned up. No Dockerfiles, no docker-compose configs.

Tech: Go CLI/daemon, Docker SDK, SQLite for session tracking, Next.js dashboard.

Use case: testing tools without polluting your host machine, reproducing bugs in clean environments, quick experiments.

Free: 3 containers/day. Pro ($4.99/mo): unlimited.

Code: https://github.com/mvtrdmqv88-droid/sandbox-cli
