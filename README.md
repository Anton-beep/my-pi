# pi agent config

Personal pi agent directory: settings, skills, gitignored secrets/sessions.

## Setup on a new machine

```bash
# Remove old pi files
rm -rf ~/.pi/agent

# Clone this repo
git clone git@github.com:USER/pi-agent.git ~/.pi/agent

# Restore secrets (not in git)
cp ~/.pi/agent.bak/auth.json ~/.pi/agent/   # or run `pi` and re-auth
```
