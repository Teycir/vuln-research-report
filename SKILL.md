---
name: vuln-research-report
description: >-
  Template and workflow for packaging vulnerability research into a public disclosure repo.
  Use when user says "create a vuln report repo", "package this finding", "disclosure repo",
  "vuln research template", "report repo", or asks to structure PoC/evidence/submission
  material for EverOS/mem0-style coordinated disclosure (VulnCheck + Vulners CNA + vendor email).
---

# Vulnerability Research Report Repo

Canonical layout and workflow distilled from the EverOS (`everos-cors-file-exfil`) and mem0
(`mem0-tenant-isolation`) disclosure repos. Copy `templates/repo-skeleton/` to start a new one.

## Repo layout

```
<target>-<slug>/                  # e.g. everos-cors-file-exfil, mem0-tenant-isolation
├── README.md                    # public index: findings table, severity/CVSS, status, timeline,
│                                #   + REQUIRED plain-language sections: "The bug, simply",
│                                #   "Attack path" (diagram), "Potential harm"
├── LICENSE                      # MIT, "Copyright (c) <year> Teycir"
├── report.md                    # master report (F-01 style) OR audit-YYYY-MM-DD/report.md for multi-finding audits
├── audit-YYYY-MM-DD/            # multi-finding audits: report.md + evidence/ + pocs/ + SHA256SUMS
│   ├── evidence/                # raw transcripts, listener logs, sqlite dumps
│   ├── pocs/                    # runnable scripts (poc.sh, stub servers, exploit.py)
│   └── SHA256SUMS
├── disclosure/
│   ├── contact-log.md           # single source of truth: every outbound contact, Message-ID, sha256
│   └── sent/                    # vendor .eml archives
├── advisory/                    # vendor addenda + sent/ .eml
├── submission/
│   ├── vulncheck/               # form-answers.md, details-<ID>.md, timeline.md, *.cve.json, SHA256SUMS
│   └── vulners/                 # email-to-cna*.txt drafts + sent/*.eml + SHA256SUMS
└── poc/ or evidence/            # top-level PoC when single-finding
```

## ID scheme

`<TARGET>-YYYY-NN` (e.g. `EVEROS-2026-02`, `MEM0-2026-01`). Never reuse; hardening notes get
`—` rows in the README table, not IDs.

## report.md / audit report structure

1. Metadata table: target, commit SHA, engagement dir, generated UTC, scope, status, disclosure line
2. Executive summary: background, posture, risk profile table (ID | Severity | CVSS | CWE | Title), recommendation summary
3. Scope & methodology (PTES-style; note non-destructive constraints)
4. Findings detail per ID:
   - Severity + full CVSS 3.1 vector + first.org calculator link + CWE
   - Affected product (repo @ commit, config preconditions)
   - Summary / root cause with `file:line` refs
   - Verified (live, UTC timestamp) — request/response transcripts
   - Impact, affected demographic
   - Steps to reproduce (minimal curl + pointer to public repo)
   - Evidence artifacts table with sha256
   - Recommended fix, references (CWE, OWASP API)
5. Hardening notes section (separate from tracked findings)
6. Disclosure status per finding

## CVSS conventions used

- Always give the vector string AND the calculator URL.
- Note alternate vectors when scope/UI assumptions change the score (e.g. "8.6 if UI:N").
- Browser-mediated reachability via permissive CORS → `UI:R`; pure local → `UI:N`; `PR:N` when no auth.
- `S:C` when the vuln crosses a security authority boundary (EverOS file exfil/SSRF did).

## Disclosure workflow (order matters)

**CVE speed rule: direct CNA submission does NOT depend on the maintainer; a GHSA does.**
A GHSA only becomes a CVE when the maintainer publishes the advisory — unresponsive
maintainers stall it indefinitely, and ghosting costs them nothing while publishing costs
them reputation. Observed: mem0's GHSA sat in `triage` >1 week with a ghosting vendor while
supergateway/agentic-radar got CVEs within 24 h via direct VulnCheck submission (no GHSA).
An open GHSA may even cause a CNA to defer to the advisory path — so file it for vendor
contact/credit, not for CVE speed.

1. **Vendor first** — check `SECURITY.md` for channel/ack window. Email from ProtonMail, body-only,
   attach evidence zip, record zip sha256. Save `.eml` to `disclosure/sent/` or `advisory/sent/`.
   GHSA counts as vendor *contact* when it's their preferred channel — but treat it as
   vendor-relations, not your CVE path.
2. **CNA immediately after** — VulnCheck form AND/OR Vulners email; do not wait on vendor or GHSA.
   Dual-CNA is net-positive: two queues, built-in duplicate checks (declare the dual
   submission in both bodies so each CNA knows to check the other), worst case a rejected
   duplicate request.
3. **Follow up vendor** if no ack near their deadline; cite their published policy.
4. **VulnCheck** — web form. Field answers live in `submission/vulncheck/form-answers.md`.
   Record case ID immediately in `timeline.md`, `contact-log.md`, `form-answers.md`, README.
5. **Vulners CNA** — email `cna@vulners.com`, subject
   `CVE request — <Product>: <title> (<Severity>, CVSS <score>)`, body = details markdown
   (see `templates/repo-skeleton/submission/vulners/email-to-cna.txt`).
6. **MITRE** — only if vendor/CNAs stall.
7. **Public repo** — flip public AFTER submissions are sent (evidence links must resolve for CNAs).
   This also starts the public-disclosure clock — note it in timeline.md.

## Email archival discipline

- User sends via ProtonMail, pastes sent body back, drops `.eml` in `~/Téléchargements`.
- Agent copies to `sent/` with UTC timestamp name: `YYYYMMDDTHHMMSSZ-<channel>-<slug>.eml`
  (convert ProtonMail `+01:00` local timestamps to UTC).
- Record in `contact-log.md`: Message-ID, Date, sha256 (short form ok in table).
- Write `SHA256SUMS` in each `sent/` dir and each `submission/*/` dir; refresh after edits.
- Duplicate downloads happen (same Message-ID, different sha256) — check Message-ID before archiving.

## GitHub repo hygiene (do before/at public flip)

- **REQUIRED — every disclosure repo must have all three, verified before public flip:**
  1. `LICENSE` file at repo root = MIT, `Copyright (c) <year> Teycir`
  2. README ends with a `## License` section linking `[MIT License](LICENSE)`
  3. GitHub repo description + topics set: `gh repo edit --description "..." --add-topic ...` — topics used so far:
  `security vulnerability-research cve responsible-disclosure bug-bounty` + per-target tags
  (`everos fastapi ssrf idor cors`, `mem0 broken-access-control cwe-639`, `supergateway websocket csrf dos`)
- README leads with the vuln table; status column tracks VulnCheck case IDs + "Vulners CNA emailed".
- **REQUIRED — every README must include three plain-language sections** (no jargon walls):
  1. *The bug, simply* — 2-4 sentences a non-specialist understands: what the code does, what it fails to check, why that matters
  2. *Attack path* — ASCII/diagram of the exploitation chain (input → missing check → impact), plus the realistic trigger (e.g. prompt injection)
  3. *Potential harm* — concrete impact bullets: what data/code/secrets an attacker gets, who is exposed

## Hardening notes vs findings

Items that are real but weak/borderline-design (missing Origin check when there's no auth,
missing rate limits, path disclosure) go in the report's hardening section and README `—` rows.
Do NOT submit them to CNAs — risk of "informational" bounce and it dilutes the real findings.

## Pre-submission checklist

- [ ] PoC rerunnable from clean clone (setup script + assertion-gated exploit)
- [ ] Every evidence file sha256'd; `SHA256SUMS` manifests regenerated after any edit
- [ ] Re-verified unfixed at upstream HEAD; record HEAD SHA + date in report
- [ ] CVSS vector calculator-verified; alternates noted
- [ ] Vendor contacted first; deadline math documented in timeline.md
- [ ] VulnCheck case IDs logged everywhere they appear
- [ ] `.eml` archives + Message-IDs in contact-log.md
- [ ] Repo public, LICENSE (MIT) file + README `## License` section + GitHub description/topics all set
