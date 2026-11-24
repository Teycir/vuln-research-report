# <Product> — <headline finding> (<TARGET>-YYYY-NN)

## Report metadata

| Field | Value |
|---|---|
| Target | <product> @ `<commit-sha>` (<date>) |
| Engagement dir | `<path>` |
| Generated (UTC) | <ISO-8601> |
| Scope | <components tested> |
| Status | FINAL — all claims reproduced live on <date> |
| Disclosure | Vendor contacted <date/channel>; VulnCheck case `<id>`; Vulners emailed <date>; repo public <date> |

## 1. Executive Summary

### 1.1 Background
<What was assessed, objective, deployment model.>

### 1.2 Overall Posture
<2-4 sentences: what holds, what doesn't, systemic vs symptomatic.>

### 1.3 Risk Profile

| ID | Severity | CVSS 3.1 | CWE | Title |
|----|----------|----------|-----|-------|
| YYYY-01 | <sev> | <score> | CWE-### | <title> |

### 1.4 Recommendation Summary
<numbered P0/P1/P2 fixes>

## 2. Scope & Methodology
- In-scope / out-of-scope / constraints (non-destructive testing notes)
- Methodology: static review → live build → exploitation → evidence → reporting

## 3. Findings Detail

### <TARGET>-YYYY-NN — <title>

- **Severity:** <High/Medium/Low>
- **CVSS 3.1:** <score> — https://www.first.org/cvss/calculator/3.1#CVSS:3.1/<vector>
  - <alternate vectors if UI/PR/AC assumptions differ>
- **CWE:** CWE-### (<name>)
- **Affected product:** <repo @ commit, config preconditions>
- **Verified at (UTC):** <timestamps>

#### Issue Description
<root cause with file:line references>

#### Verified (live)
| Request | Result |
|---|---|
| `<request>` | `<result>` |

#### Impact
<confidentiality/integrity/availability consequences, affected demographic>

#### Steps to Reproduce
```bash
<minimal curl/script; pointer to public repo for full PoC>
```

#### Evidence artifacts
| Path | sha256 |
|---|---|
| `evidence/<file>` | `<hash>` |

#### Recommended Fix
<concrete remediation>

#### References
- CWE-###: <url>
- OWASP API#: <url>

## 4. Hardening notes (not submitted)
| Item | Severity | CVSS | Note |
|---|---|---|---|
| <item> | <sev> | <score> | <why not a tracked finding> |

## 5. Disclosure status
- Reporter: Teycir. LLM-assisted analysis/drafting; verified live.
- Vendor: <channel, dates, Message-IDs, ack status>
- VulnCheck: <case IDs + dates>
- Vulners CNA: <send dates + Message-IDs>
- GHSA: <id if any>
- Public repo: <url + public-since timestamp>
