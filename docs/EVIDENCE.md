# Evidence Register — Sanitized Screenshots

All PNGs in `evidence/` are **cropped from screenshots supplied during the live November 7, 2026 lab session**. Cropping removes unrelated windows, browser tabs and account details; command results are not fabricated or retyped into the screenshot images. Where raw screenshots included private IPs and user environment details, publication is limited to focused proof excerpts. Sanitized images remain genuine screenshots, not simulated terminal output.

| File | Shows | Interpretation |
| --- | --- | --- |
| [01 — Ansible ping](../evidence/01-ansible-ping.png) | Successful `pong` | Remote Python execution over SSH |
| [02 — Initial deployment](../evidence/02-initial-deployment.png) | Nginx install/page/service and recap | Config management ran with no failures |
| [03 — Firewall automation](../evidence/03-firewall-automation.png) | HTTP firewalld task and recap | Rule rollout recorded as a change |
| [04 — HTTP validation](../evidence/04-http-validation.png) | Nginx response headers / HTML excerpt | HTTP response is `200 OK` |
| [05 — Browser proof](../evidence/05-webpage.png) | Rendered deployed static homepage | Browser rendered the page from lab server |
| [06 — Idempotency](../evidence/06-idempotency.png) | Repeat execution with `changed=0` | Desired state held across rerun |

**Limitations:** Evidence shows one managed target; no production uptime, CI/CD pipeline, TLS, horizontal scaling, or compliance audit is claimed. Browser and HTTP captures deliberately omit the address bar/host value to protect lab addressing. The same results can be reproduced by following the SOP and using your own managed-node address.

## Full session archive

See [Screenshot Timeline](SCREENSHOT-TIMELINE.md) for the complete **31-screenshot** private session index. Original desktop screenshots are deliberately ignored by Git and must not be publicly uploaded without individual redaction.
