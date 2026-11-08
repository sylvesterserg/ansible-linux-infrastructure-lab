# Publish from Cursor — without losing the validated source

**Preferred workflow:** push from the real controller checkout. The code in this documentation package is a faithful reconstruction of the shown deployment, but the actual controller playbook is authoritative. **Do not blindly overwrite a working file.**

## 1. Move documentation/evidence to the controller

Download and unzip the archive on your Mac. With the controller reachable from your Mac over SSH, copy only the docs, screenshots and safe supporting files first:

```bash
# RUN FROM YOUR MAC TERMINAL, from inside the extracted package:
tar -cf - docs evidence README.md requirements.yml .gitignore inventory/hosts.ini.example | \
  ssh ansible@192.168.0.34 "cd ~/cloudops-lab/ansible-linux-fleet && tar -xf -"
```

The package's playbook and `ansible.cfg` may be compared with your **live** versions, not overwritten. The old `README.md` is not expected to be present, but check first. If copying from your Mac isn't convenient, use Cursor Remote SSH Explorer to transfer the files.

## 2. Configure Git identity (the screenshot showed this error)

```bash
# RUN ON THE CONTROLLER in the project directory:
cd ~/cloudops-lab/ansible-linux-fleet
git config user.name "Your GitHub display name"
git config user.email "YOUR_GITHUB_VERIFIED_EMAIL"
git branch -M main
```

Use your own verified GitHub email or GitHub noreply address. Do not copy placeholder values literally.

## 3. Review sensitive data and stage intentionally

```bash
cat .gitignore
# Ensure inventory/hosts.ini, .env, credentials, keys and logs are ignored.
git status --short
git add README.md ansible.cfg .gitignore requirements.yml \
  playbooks/webserver.yml inventory/hosts.ini.example docs/ evidence/
git diff --cached --stat
git diff --cached --check
git status --short
```

If `.gitignore` does not exist yet, copy it from the package first. **Verify** that `inventory/hosts.ini` is not staged and that screenshots reveal no IPs or keys. Before any public push, audit `git diff --cached`.

## 4. Choose your repository destination

Option A: create a new **public** repository named `ansible-linux-infrastructure-lab` through GitHub's UI, then set its remote:

```bash
git remote -v
git remote add origin git@github.com:YOUR_ACCOUNT/ansible-linux-infrastructure-lab.git
# If origin already exists, inspect it and use git remote set-url origin ... only when intentional.
git commit -m "docs: publish validated Ansible CloudOps lab and SOP"
git push -u origin main
```

Option B: add the project as a new subdirectory to an existing toolkit repository. That requires organizing the files into the target subdirectory and must not be done with a blind root-level push. Confirm the repository URL, permissions, and default branch first.

The earlier `git push` failed because no remote was configured; `git commit` failed because no author identity was configured. A GitHub account connection here does not automatically authenticate the controller's Git CLI. Set up either SSH key authentication for your GitHub account or `gh auth login` where available. Never paste a GitHub token into a repository file.

## 5. Validate GitHub

Confirm that the README renders, embedded screenshot links resolve, SOP is readable, sensitive local inventory is absent, and the clone instructions work. Pin the repository on your GitHub profile for recruiters.
