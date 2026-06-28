# Hyperium ZERO-STATE v0.1.0 - Pre-Engagement Integrity Validator

**Author:** Patricio Tirado - CEO, Hyperiumia
**Contact:** hyperiumia@protonmail.com
**Language:** Rust 2021 Edition
**Standard:** ISO/IEC 27037:2012
**Status:** v0.1.0 MVP - Windows + Linux

---

## What Is ZERO-STATE?

A Pre-Engagement Integrity Validator that certifies the initial state of a system before any offensive engagement begins. It produces an immutable, SHA-256 sealed Digital Notarized Act that proves whether a system was clean or already compromised before the Red Team touched it.

If a client suffers an incident during or after an audit, ZERO-STATE proves you didn't bring it.

## The Problem It Solves

No pentester in the market has a tool that cryptographically certifies "I didn't bring this." If a client suffers an incident during or after an audit, without ZERO-STATE the responsibility falls by default on the evaluator. That is an enormous professional, legal and reputational risk.

## How It Works

1. Client signs contract
2. zero-state scan --client "Empresa SA" --authorized-by "CISO"
3. ZERO-STATE scans in 3-5 minutes
4. Result A: CLEAN -> Sealed Certificate of Zero State (SHA-256) -> RED-CORE proceeds
   Result B: COMPROMISED -> Dictamen of Prior Compromise -> Engagement changes to DFIR

## 4 Modules

| Module | Checks | Purpose |
|--------|:------:|---------|
| persistence_audit | 8-10 | Registry, tasks, services, cron, systemd, WMI, SSH keys, LD_PRELOAD |
| network_state | 3 | Active connections, listening ports, promiscuous interfaces |
| integrity_check | 2 | System binary hashing, config file integrity, driver signatures |
| memory_scan | 3 | Process enumeration, hidden process detection, injection indicators |

## CLI Commands (6)

- zero-state info - System info
- zero-state scan - Full 4-module integrity scan
- zero-state persistence - Persistence audit only
- zero-state network - Network state only
- zero-state integrity - Integrity check only
- zero-state memory - Memory scan only

## Acta Notarial Digital (ISO/IEC 27037)

The scan produces a Digital Notarized Act with:
- Verdict: CLEAN / SUSPICIOUS / COMPROMISED
- Evidence chain: SHA-256 tamper-evident (inherited from RED-CORE)
- Legal declaration: ISO/IEC 27037:2012 compliant
- IoC collection: All indicators from all 4 modules
- Dictamen hash: SHA-256 of the complete document

## Architecture

- Air-Gapped: No outbound network connections
- Static YARA rules: Compiled into binary
- No dependencies: Single executable, zero runtime deps
- Inherited from RED-CORE: Evidence chain, types, SHA-256 engine

## Integration with RED-CORE

ZERO-STATE certifies entry -> RED-CORE executes -> RED-CORE certifies exit

ZERO-STATE does not compete with RED-CORE: it protects it. RED-CORE acts on the system; ZERO-STATE certifies the state before RED-CORE touches it.

## Tests

46 unit tests across 7 modules, all passing.

## Platform Support

| Platform | v0.1.0 | v0.5.0 |
|----------|:------:|:------:|
| Windows | YES | YES |
| Linux | YES | YES |
| macOS | - | Planned |

## Roadmap

| Version | Status | Content |
|---------|:------:|---------|
| v0.1.0 | CURRENT | Persistence + Network + Integrity + Memory |
| v0.5.0 | PLANNED | macOS + YARA-X + hardware audit |
| v1.0.0 | PLANNED | Full dictamen engine + REST API + Docker |

---

Source code is private. Demo available upon request.

Hyperiumia - https://github.com/hyperiumia/hyperium-zero-state
