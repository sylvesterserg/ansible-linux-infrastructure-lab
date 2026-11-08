# Full Screenshot Timeline — Local Archive

This project was documented with **31 original screenshots** from the live November 6–7, 2026 build. They are included in `private-originals/` for your personal reference, **not for public GitHub publication**. They may reveal LAN addresses, SSH information, personal browser tabs, or host details. Git ignores that directory.

The six curated screenshots in [`evidence/`](../evidence/) are the public-facing proof excerpts. These preserve successful deployment outcomes without exposing an entire desktop session.

| # | Local archival screenshot | Milestone |
|---:|---|---|
| 01 | `01-20261106-134718.png` | Proxmox environment and old controller |
| 02 | `02-20261106-154158.png` | Existing Ubuntu controller and APT stall |
| 03 | `03-20261106-154238.png` | Controller network addressing and gateway |
| 04 | `04-20261106-164315.png` | LXC creation and template selection |
| 05 | `05-20261106-164748.png` | Selecting Ubuntu templates |
| 06 | `06-20261106-214554.png` | Ubuntu 26.04 controller and initial APT issue |
| 07 | `07-20261106-215101.png` | DNS inspection in LXC |
| 08 | `08-20261106-215118.png` | Gateway connectivity verified |
| 09 | `09-20261106-215139.png` | Internet IP connectivity verified |
| 10 | `10-20261106-215205.png` | DNS lookup diagnosis |
| 11 | `11-20261106-221238.png` | Ansible installation verified |
| 12 | `12-20261106-222607.png` | Ansible workspace directories created |
| 13 | `13-20261107-141221.png` | Remote SSH host identity and login attempt |
| 14 | `14-20261107-141333.png` | SSH daemon status and fingerprint |
| 15 | `15-20261107-202746.png` | Controller SSH connection and config |
| 16 | `16-20261107-203658.png` | Rocky Linux host OS and address discovery |
| 17 | `17-20261107-204911.png` | Inventory creation context |
| 18 | `18-20261107-205010.png` | Inventory configured |
| 19 | `19-20261107-205216.png` | Inventory discovery output |
| 20 | `20-20261107-205303.png` | SSH connection to Rocky Linux node |
| 21 | `21-20261107-205508.png` | SSH public key deployed and Ansible ping successful |
| 22 | `22-20261107-214651.png` | First playbook deployment result |
| 23 | `23-20261107-214820.png` | Browser connection issue prior to firewall change |
| 24 | `24-20261107-215240.png` | Editing firewall automation task |
| 25 | `25-20261107-215531.png` | Firewall status and Ansible collection |
| 26 | `26-20261107-215848.png` | Adding firewalld Python dependency |
| 27 | `27-20261107-220040.png` | Firewall automation successful |
| 28 | `28-20261107-220211.png` | HTTP 200 response from Mac terminal |
| 29 | `29-20261107-220253.png` | Web page visible in browser |
| 30 | `30-20261107-220529.png` | Idempotent second playbook execution |
| 31 | `31-20261107-221313.png` | Git repository initial setup and missing identity/remote |

## Publishing safely

1. **Do not commit `private-originals/`**. These are complete raw screenshots preserved only inside the downloadable ZIP and local working copy.
2. Publish selected images from `evidence/`, after a final personal review of each image.
3. Confirm ignored files with `git status --ignored --short` and inspect staged files with `git diff --cached --name-only`.
4. Screenshots demonstrate one test VM, not production operations.

## GitHub evidence sequence

- Controller and SSH established
- Ansible inventory and `ping: pong`
- First Nginx deployment
- Firewalld HTTP rule applied
- HTTP 200 and browser rendering
- Idempotency rerun with `changed=0`
