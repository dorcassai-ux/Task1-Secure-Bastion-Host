# Secure Linux Bastion Host and Process Management

Task 1 of the Elevvo Cloud and DevOps Engineering Internship, completed by Dorcas Torshie Sai in October 2026.

## Overview

I turned a fresh Ubuntu virtual machine into a small, hardened bastion host. SSH now accepts only key-based logins, root login is switched off, a web service runs under systemd, and in the bonus task SSH listens only on a private Tailscale address.

## What I did

- Worked entirely from the command line and set up the project structure
- Generated an Ed25519 SSH key pair with the correct file permissions
- Hardened `sshd_config`: no root login, no password authentication, public key only
- Deployed a Python web service on port 8000 and managed it with `systemctl` (start, stop and enable on boot)
- Bound SSH to the loopback and Tailscale addresses, so port 22 is not reachable from the public network
- Verified that key login works and that a login without the key is refused

## Tools

Ubuntu 26.04 LTS on VirtualBox, Bash, OpenSSH, systemd, Python 3, Tailscale

## Results

| Requirement | Outcome |
|---|---|
| Ed25519 key pair with correct permissions | Completed |
| Root login and password authentication disabled | Completed |
| Web service managed with systemd, enabled on boot | Completed |
| Key login works, password login refused | Completed |
| Bonus: SSH restricted to a private interface | Completed |

## Highlights

**Key login succeeds and a login without the key is refused**

<img src="screenshots/11-key-login-verification.png" alt="Key-based login succeeds and a login without the key is refused" width="700">

**SSH listens only on the loopback and Tailscale addresses**

<img src="screenshots/bonus-3-ssh-bound-to-private-interface.png" alt="SSH bound to the loopback and Tailscale addresses only" width="700">

**The web service is enabled and responding**

<img src="screenshots/07-enabled-and-curl-test.png" alt="The web service is enabled and responds to curl" width="700">

## Quick reference

```bash
ssh-keygen -t ed25519 -f ~/.ssh/bastion_key       # create the key pair
sudo sshd -t                                      # test the SSH config before restarting
sudo systemctl enable --now myweb.service         # start the service now and at boot
sudo ss -tulnp | grep :22                         # check where SSH is listening
```

## Repository contents

```
.
├── README.md                  # this summary
├── FULL-REPORT.md             # step-by-step documentation with every command explained
├── Task1-Secure-Bastion-Host-Documentation.docx   # Word version of the full report
├── configs/
│   ├── myweb.service          # systemd unit file for the web service
│   └── sshd_config.hardened   # the final SSH configuration
└── screenshots/               # evidence for each step
```

## Full documentation

- [Full report (Markdown)](FULL-REPORT.md) covers each step with a description, the commands used and explained, screenshots and notes, plus the problems I ran into
- [Full report (Word)](Task1-Secure-Bastion-Host-Documentation.docx)

## What I learned

Securing a server means closing one route of attack at a time: root login, password guessing, public exposure of the SSH port, and a service that runs with more privilege than it needs. Testing each change before relying on it caught most of my mistakes early.
