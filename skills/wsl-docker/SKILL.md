---
name: wsl-docker
description: >
  Use when Docker is unavailable inside WSL2 — "Cannot connect to the Docker daemon",
  "docker: command not found" — and containers need to run without asking the user.
---

# Start Docker in WSL

**1. Check Docker is really down**

```bash
docker info >/dev/null 2>&1 || echo "down"
```

**2. Check this is WSL**

```bash
grep -qi microsoft /proc/version && echo "WSL"
```

Not WSL → this skill does not apply; stop.

**3. Enable Docker from the Windows side**

```bash
if docker desktop --help >/dev/null 2>&1; then
  docker desktop start --timeout 180
else
  powershell.exe -NoProfile -Command "Start-Process 'C:\Program Files\Docker\Docker\Docker Desktop.exe'"
fi
```

**4. Wait for it, then continue**

```bash
for i in $(seq 1 60); do docker info >/dev/null 2>&1 && break; sleep 2; done
```

Still down after that: stop and inform the operator.
