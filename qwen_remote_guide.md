# Remote Qwen Code CLI via Code-Server

## What This Setup Does

This setup provides a full **VS Code in the browser** (via `code-server`) that is pre-configured to run the **Qwen Code CLI** (`qwen`). It is securely exposed through your Cloudflare tunnel at:

```
https://qwen.mahathasan.com
```

Instead of a simple terminal, you get a powerful IDE with a file explorer, multiple terminal tabs, and a modern editor—all pointing at your actual project files.

---

## Architecture

```
Your Browser (anywhere)
        │
        ▼
https://qwen.mahathasan.com
        │  (Cloudflare Tunnel)
        ▼
cloudflared container
        │  (internal Docker network)
        ▼
code-server container  (VS Code Web on port 8080)
        │  (volume mounts from host)
        ├── /usr/bin/qwen        ← the CLI binary (Node.js script)
        ├── /home/magus/.qwen    ← your OAuth credentials & settings
        └── /mnt/data/wd         ← your actual projects folder
```

The **code-server** container:
- Runs a custom build (Node.js 20 installed) to support the Qwen script.
- Mounts your host's `qwen` binary and `~/.qwen/` credentials.
- Mounts `/mnt/data/wd` to `~/projects` so you can code on your real files.
- Uses a password gate (`qwen2024`) for initial access.

---

## Technical Details

### Cloudflare Routing (`config.yml`)
```yaml
ingress:
  - hostname: qwen.mahathasan.com
    service: http://code-server:8080
```

### Docker Service (`docker-compose.yml`)
```yaml
code-server:
  image: my-code-server:latest   # Custom build with Node.js
  container_name: code-server
  environment:
    - PASSWORD=qwen2024
  volumes:
    - /usr/bin/qwen:/usr/bin/qwen:ro
    - /home/magus/.qwen:/home/coder/.qwen
    - /mnt/data/wd:/home/coder/projects
```

---

## How to Use

1.  Open **https://qwen.mahathasan.com**
2.  Log in with password: `qwen2024`
3.  Open the integrated terminal (**Ctrl + `**)
4.  Navigate to your projects: `cd ~/projects`
5.  Run `qwen` as usual.

---

## Maintenance

### Rebuild and Update
If you need to update the environment (e.g., install new tools):
```bash
cd /mnt/data/wd/Automation
# Rebuild the custom Node.js image
docker build -t my-code-server - < Dockerfile.code-server
# Recreate the container
docker compose up -d --force-recreate code-server
```

### Logs & Status
```bash
docker logs code-server -f
docker compose ps code-server
```

---

## Security

> [!CAUTION]
> Code-server provides full file system and terminal access. 
> 1. **Change the password**: Edit the `PASSWORD` in `docker-compose.yml`.
> 2. **Use Cloudflare Access**: It is **highly recommended** to place `qwen.mahathasan.com` behind a Cloudflare Zero Trust gateway (Google Login / GitHub OAuth / One-Time PIN).
