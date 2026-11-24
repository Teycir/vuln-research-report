# vuln-research-report

**A complete template and workflow for packaging vulnerability research into a public, coordinated-disclosure repository.**

Distilled from real disclosure engagements — EverOS (`everos-cors-file-exfil`), mem0 (`mem0-tenant-isolation`), and Supergateway (`supergateway-security-research`) — where findings moved through vendor notice → VulnCheck → Vulners CNA → public evidence repo with full audit trails.

Use it as an agent skill (`SKILL.md`) or as a copy-paste skeleton (`templates/repo-skeleton/`).

## What it gives you

A disclosure repo that looks like it was run by a professional VRP team:

```
<target>-<slug>/
├── README.md                  # findings index: severity, CVSS, CNA status
├── LICENSE                    # MIT
├── report.md                  # master report — metadata, exec summary, findings detail
├── audit-YYYY-MM-DD/          # multi-finding audits: report + evidence + pocs + SHA256SUMS
├── disclosure/
│   ├── contact-log.md         # every outbound contact: Message-ID, sha256, status
│   └── sent/                  # archived vendor .eml files
├── advisory/                  # vendor addenda + sent .eml
├── submission/
│   ├── vulncheck/             # form answers, per-finding details, timeline, case IDs
│   └── vulners/               # CNA email drafts + sent .eml + SHA256SUMS
├── evidence/                  # raw transcripts, listener logs, ground-truth dumps
└── poc/                       # runnable, assertion-gated exploit scripts
```

## Workflow it encodes

1. **Vendor first** — honor `SECURITY.md` channel and ack window; attach evidence zip; archive the `.eml`.
2. **VulnCheck** — web form; log the case ID everywhere it appears.
3. **Vulners CNA** — `cna@vulners.com`, subject `CVE request — <Product>: <title> (<sev>, CVSS <score>)`.
4. **MITRE** — only if the others stall.
5. **Public flip** — after submissions are sent, so evidence links resolve for CNAs.

Plus the discipline that makes the trail defensible: UTC timestamps everywhere, Message-IDs and sha256 for every sent email, `SHA256SUMS` manifests per directory, CVSS vectors with calculator links and alternate-scenario scores, and a hard line between *tracked findings* (submitted) and *hardening notes* (documented, not submitted).

## Usage

**As an agent skill** — symlinked into `~/.agents/skills/`, `~/.claude/skills/`, and `~/.config/devin/skills/`; invoke `vuln-research-report` when packaging a finding.

**As a template** — copy the skeleton and fill in:

```bash
cp -r templates/repo-skeleton ~/Repos/<target>-<slug>
cd ~/Repos/<target>-<slug> && git init
```

Every file is a working template with placeholders (`<TARGET>-YYYY-NN`, `<commit-sha>`, `<uuid>`), not just documentation.

## Provenance

Patterns verified across three live engagements:

| Repo | Findings | Outcome |
|---|---|---|
| [everos-cors-file-exfil](https://github.com/Teycir/everos-cors-file-exfil) | 3 (file:// exfil 8.2, SSRF 7.4, IDOR 6.8) | VulnCheck + Vulners submitted, vendor ghosted |
| [mem0-tenant-isolation](https://github.com/Teycir/mem0-tenant-isolation) | 2 (tenant authz bypass 8.8, /configure disclosure 4.3) | VulnCheck + Vulners + GHSA `GHSA-62c5-2wrq-ffpv` |
| [supergateway-security-research](https://github.com/Teycir/supergateway-security-research) | 2 (DoS, CSWSH) | CVEs + GHSAs published |

## License

MIT
