# Troubleshooting Runbook

## APT hangs on Ubuntu controller

**Observed:** `apt update` stalled at `Connecting to archive.ubuntu.com` on more than one Ubuntu version. The container could ping the LAN gateway and a public IP, but name resolution appeared blocked. `/etc/resolv.conf` had Tailscale MagicDNS nameservers inherited from the host. **Resolution:** container-specific DNS was updated to reachable public resolvers through Proxmox; package installation then succeeded. **Lesson:** verify gateway, raw IP reachability and name lookups separately. Avoid assuming an OS reinstall fixes a shared DNS configuration issue.

```bash
ip -br addr
ip route
cat /etc/resolv.conf
ping -c 3 1.1.1.1
getent ahostsv4 archive.ubuntu.com
```

Do not rewrite host-wide DNS just to repair one container; preserve MagicDNS where required for other services.

## SSH host-key changed after rebuilding a container

**Observed:** Mac SSH warned that remote host identification changed after LXC regeneration. **Response:** independently confirm the ED25519 host-key fingerprint from Proxmox console, then remove only the obsolete host entry using `ssh-keygen -R <host-or-ip>` as appropriate and reconnect. Never disable strict host-key checking as a shortcut.

## SSH password / user mismatch

**Observed:** SSH daemon was running, but attempts as `root` failed. **Resolution:** connect as the dedicated `ansible` account; verify user credentials and authorized keys. `ssh -v ansible@<host>` helps identify failures. Manage systemd services as root or with legitimate sudo access, not as an unprivileged user without elevation.

## Ansible reports no hosts matched

**Observed:** `ansible all -m ping` initially reported the inventory file could not be parsed because it did not yet exist. **Resolution:** create ignored `inventory/hosts.ini`, then verify with `ansible-inventory --graph`. Never paste your shell prompt as a command.

## Nginx works locally but browser cannot connect

**Observed:** playbook successfully checked `http://127.0.0.1/`, while remote browser access initially failed. **Cause identified:** firewalld was active and HTTP access was not allowed. **Resolution:** install `python3-firewall` and configure `ansible.posix.firewalld` with `service: http`, `permanent: true`, `immediate: true`, `state: enabled`. Verify active zone policy; do not disable the firewall globally.

```bash
# Read-only diagnosis, use authorized permissions:
systemctl is-active firewalld
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --list-services
curl -i http://127.0.0.1/
```

## Idempotency re-run reports changed tasks

Inspect the task marked changed, compare the intended state to the target, and determine whether drift or a non-idempotent action is responsible. `--check --diff` can help, though modules may not fully model all changes in check mode. In the validated run every task was stable (`changed=0`).
