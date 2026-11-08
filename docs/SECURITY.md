# Security and Public-Repository Checklist

This portfolio repo must not expose operational access details.

- [ ] `git status --short` shows no private `inventory/hosts.ini` or credential files
- [ ] `git diff --cached` is reviewed before commit; stage named files or the isolated package only
- [ ] No passwords, tokens, private SSH keys, auth cookies, Tailscale configuration, internal DNS suffixes or recovery information
- [ ] Screenshots are cropped to exclude browser tabs, account identities, IP addresses, private infrastructure names, host key fingerprints, and key material
- [ ] All IP addresses in examples use documentation-only RFC 5737 address space (`192.0.2.0/24`)
- [ ] Public `ansible.cfg` retains `host_key_checking = True`
- [ ] Project is explicitly marked lab-only, not Internet-exposed production architecture
- [ ] Only the public SSH key is ever distributed to targets; no real public or private key material belongs in this repo

Recommended before pushing:

```bash
git status --short
git diff --cached --stat
git diff --cached --check
git diff --cached
```

A `.gitignore` cannot remove data already tracked by Git. If secrets were accidentally committed, rotate them and purge history using a reviewed remediation process before making the repository public.
