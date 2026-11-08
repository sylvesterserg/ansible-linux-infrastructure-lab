# SOP-001 — Deploy and Validate Ansible-Managed Rocky Linux Web Server

**Owner:** Infrastructure Operations / Sylvect Tech LLC
**Version:** 1.0 · **Verified lab date:** 2026-11-07
**Scope:** Disposable Rocky Linux test VM under Proxmox, controlled from Ubuntu LXC.
**Change class:** Lab deployment only. **Not approved for production use.**

## 1. Purpose and acceptance criteria

Deliver Nginx and a static HTML homepage, authorize HTTP in host firewalld, and prove re-runs do not change an already compliant target. Accept only when: Ansible ping succeeds; syntax check succeeds; deployment recap has `failed=0` and `unreachable=0`; local HTTP assertion passes; external `curl` gets `HTTP 200`; second run has `changed=0`.

## 2. Preconditions

- Test VM is **disposable**, has a unique identity, and is reachable from the controller.
- Ubuntu controller has Ansible and SSH client; Rocky guest has Python, openssh-server, sudo, and firewalld enabled.
- A dedicated remote `ansible` account exists with approved sudo access; controller's **public** ED25519 key is authorized on the target.
- The target SSH host key is checked against console output before first trust.
- Managed-host IP is recorded **only** in ignored `inventory/hosts.ini`.
- A tested Proxmox snapshot/backup or ability to recreate the disposable guest exists.
- Controller can download collection dependencies; no secrets are committed.

## 3. Deployment procedure

From the **controller**, in the project root:

```bash
ansible --version
ansible-galaxy collection install -r requirements.yml
cp -n inventory/hosts.ini.example inventory/hosts.ini
# Edit the example IP in the ignored inventory file.
ansible-inventory --graph
ssh ansible@<actual-target-ip>
# Exit after verifying key-based access and host identity.
ansible linux_servers -m ping
ansible-playbook playbooks/webserver.yml --syntax-check
ansible-playbook playbooks/webserver.yml --ask-become-pass
```

**Security note:** `--ask-become-pass` accepts the remote account's sudo password interactively, not in source control. Do not disable host-key checking to circumvent an identity mismatch.

## 4. Functional verification

```bash
ansible linux_servers -m ping
curl -i --connect-timeout 5 http://<actual-target-ip>/
```

Confirm the page contains `CloudOps Infrastructure Automation`, HTTP status is `200`, and firewalld allows the `http` service. A local `127.0.0.1` test alone does not prove client reachability. If the client fails, inspect the active firewall zone and network path.

## 5. Idempotency check

```bash
ansible-playbook playbooks/webserver.yml --ask-become-pass
```

Expected steady state: `changed=0`, `failed=0`, `unreachable=0`. The historical validated result was `ok=8 changed=0 unreachable=0 failed=0`. Save a sanitized recap in evidence when rerun.

## 6. Rollback (lab target only)

Preferred: restore the disposable VM snapshot or rebuild from template. For a narrower rollback, remove the managed service and HTTP rule only if no other application depends on them; verify existing consumers and firewall policy before removal. Do not turn off firewalld globally. Note that replacing `/usr/share/nginx/html/index.html` overwrites an existing page; target only a dedicated demo guest.

## 7. Change record

| Date | Change | Verification |
| --- | --- | --- |
| 2026-11-07 | Initial Ansible Nginx deployment | `ok=6 changed=3 failed=0` |
| 2026-11-07 | Enabled HTTP in firewalld | `ok=8 changed=1 failed=0` |
| 2026-11-07 | External HTTP and re-run validation | HTTP `200 OK`; `ok=8 changed=0 failed=0` |

## 8. Escalation

If SSH, sudo, DNS, firewalld, or HTTP checks fail, see [TROUBLESHOOTING.md](TROUBLESHOOTING.md). Stop automated changes if the target's identity or environment is uncertain.
