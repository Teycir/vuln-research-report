# Disclosure contact log — <Product> <headline> (<TARGET>-YYYY-NN)

Target: <product> @ `<commit>`
Channel: <primary per SECURITY.md; note if private vuln reporting disabled>

| # | Contact | Address | Purpose | Status | Date (UTC) | Reference |
|---|---------|---------|---------|--------|------------|-----------|
| 1 | <vendor> | <email> | Vendor notice + report + evidence | sent | <ISO> | Message-ID `<...>`; raw copy `sent/<file>.eml` |
| 2 | VulnCheck | web form | CVE assignment request | submitted | <date> | Case ID `<uuid>`; package `submission/vulncheck/` |
| 3 | Vulners (CNA) | cna@vulners.com | CVE ID request | sent | <ISO> | Message-ID `<...>`; raw copy `submission/vulners/sent/<file>.eml` |

## Sent package

- Attachment: `<evidence>.zip`
- Zip SHA-256: `<hash>`
- Contents: <manifest summary>
- Repo: <url> (public/private + since when)

## Vendor expectations (from SECURITY.md)

- <ack window, fix-before-disclosure policy, publishing channel>
- <threat-model caveats to be ready to reiterate>

## Watch items

- [ ] Vendor acknowledgement
- [ ] CVE/GHSA assignment — record here and update `report.md` if assigned
- [ ] Fix release — re-verify against PoC before any public disclosure
- [ ] Disclosure deadline
