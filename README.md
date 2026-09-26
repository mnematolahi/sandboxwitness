# SandBoxWitness

**Behavioral trust testing for AI-generated and AI-modified code.**

🚧 **Status: early development.** sandboxwitness is being actively built and is not yet functional. This README describes where the project is heading; it does not describe a finished tool. Watch this repo (or check the Issues/Projects tab) for progress.

---

## What is SandBoxWitness?

AI coding agents rarely write code with bad intentions — but they can still ship code that does more, or something different, than what was actually asked for: an extra dependency nobody requested, an outbound network call that isn't part of the feature, an install script that reaches further than it should. Standard code review and unit tests are built to check whether code does what it's *supposed* to do. They are not built to catch what it does *in addition*.

sandboxwitness is a **Behavioral Trust Testing** tool built specifically for that gap. It combines static analysis of a project with sandboxed runtime observation — actually running the code in an isolated container and watching what it does — so that unexpected behavior in AI-generated or AI-modified code can be caught before it reaches production.

sandboxwitness is **not** a general-purpose malware scanner and does not claim to catch every possible malicious behavior. Every finding is evidence-based and reported on a graded scale rather than a binary verdict, because most unexpected behavior turns out to be harmless — it just deserves a second look before it ships.

## Why this exists

- An AI agent can add a dependency, and that dependency's install script can do things that have nothing to do with the dependency itself.
- An AI agent can be pointed at a package name that doesn't actually exist — and an attacker can have registered that exact name in advance, betting that a model (or a developer skimming its output) won't notice.
- Code that looks completely reasonable in review can still make a network call, read a credential, or write a file somewhere it has no reason to, and none of that shows up in a green test suite.
- Static analysis and linting can't observe any of this, because none of it happens until the code actually runs.

## What sandboxwitness does (design goals)

The list below describes what the project is being built to do. See the [Roadmap](#roadmap--status) for what actually exists today.

**Static analysis**
- Dependency and lockfile diffing across ecosystems (Python first, others planned)
- External URL extraction and classification (documentation vs. API vs. unexplained destinations)
- Git-diff–aware change detection, so a scan can focus on what actually changed

**Sandboxed runtime analysis**
- Every untrusted project runs inside an isolated Docker container — never directly on the host machine
- Process, filesystem, and network activity are all instrumented and recorded
- Three network modes: fully offline by default, monitored (logged, still isolated), or allowlist-only

**Canary-based exfiltration detection**
- Inert, fake credentials and secrets are planted inside the sandbox
- If the code under test tries to read or transmit them, that's a concrete, evidence-backed finding — not a guess

**Evidence-based reporting**
- Every finding ships with the evidence behind it: the file, the line, the process, the network event
- Findings are graded (roughly: pass / informational / warning / high / critical) instead of a flat "safe" or "malicious" label
- Human-readable, JSON, and HTML report formats

## What sandboxwitness is not

- Not a replacement for normal code review, testing, or CI security scanning — it's a complementary layer
- Not a guarantee that a project has no malicious code; absence of findings means nothing suspicious was *observed*, not that nothing is *possible*
- Not a tool that auto-fixes anything — it detects, analyzes, and reports; it does not modify your code or dependencies

## Roadmap / Status

sandboxwitness is being built in stages, starting with a small, genuinely working core rather than a large surface of half-finished features:

- **Stage 1 (in progress):** static analysis for one ecosystem, a basic isolated Docker sandbox, canary-based detection, and JSON/HTML reporting.
- **Later stages:** additional language ecosystems, richer network behavior analysis, baseline/allowlist learning, and CI integration.

Nothing in this README should be read as "already available" unless it's reflected in a tagged release.

## Installation

Not yet available. sandboxwitness is pre-release — installation instructions will be added here once the first usable version ships.

## Related

A companion reference document, an **AI Code Quality, Testing & Release Verification standard**, covers the correctness/release-readiness side of AI-assisted development (formatting, testing, coverage, CI gates). sandboxwitness is meant to complement it by focusing specifically on runtime *behavior* rather than correctness. *(Link to be added once published.)*

## Contributing

Issues and design discussion are welcome. Since the architecture is still settling, please open an issue before sending a large pull request so the direction can be agreed on first.

## License

See the [LICENSE](LICENSE) file in this repository.
