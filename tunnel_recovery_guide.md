# Cloudflare Tunnel Recovery & Troubleshooting Guide

This guide explains how to restore your Cloudflare Tunnel if it breaks or is deleted, and documents the challenges faced during the initial setup to avoid them in the future.

## 🛠️ How to Re-Setup from Scratch

If you delete the `cloudflared` folder or the tunnel itself, follow these steps:

### 1. Authorize cloudflared
Ensure your `cert.pem` is in the `cloudflared/` directory. If you don't have it, run:
```bash
docker exec -it cloudflared cloudflared tunnel login
```
*Note: This will provide a link to authorize your domain.*

### 2. Create a New Tunnel
```bash
docker exec -it cloudflared cloudflared --origincert /etc/cloudflared/cert.pem tunnel create <tunnel_name>
```
*Tip: This generates a new `<UUID>.json` credentials file in the `cloudflared/` folder.*

### 3. Update Configuration
Update `cloudflared/config.yml` with the new **Tunnel ID** and **Credentials File** path.
> [!IMPORTANT]
> Always include `protocol: http2` in `config.yml` to avoid connection timeouts on port 7844.

### 4. Route DNS
```bash
docker exec -it cloudflared cloudflared --origincert /etc/cloudflared/cert.pem tunnel route dns --overwrite-dns <tunnel_name> n8n.mahathasan.com
docker exec -it cloudflared cloudflared --origincert /etc/cloudflared/cert.pem tunnel route dns --overwrite-dns <tunnel_name> portainer.mahathasan.com
```

### 5. Restart Services
```bash
docker compose up -d --force-recreate
```

---

## 🔍 Post-Mortem: Why the Initial Setup Took Time

Several technical hurdles were encountered during the transition from Token-based to Local configuration:

### 1. Permission Denied Errors
*   **The Issue:** The `cloudflared` container (running as a non-root user for security) could not read the `.json` credentials file I created.
*   **The Fix:** Updated file permissions to `644` (readable by the container user) and the directory to `777` temporarily to allow the container to write new credentials.

### 2. Connection Timeouts (Port 7844)
*   **The Issue:** By default, Cloudflare Tunnels try to use the **QUIC** protocol on port 7844 (UDP). Many local networks/firewalls block this port, causing the tunnel to hang in a "Retrying" state.
*   **The Fix:** Forced the tunnel to use **http2** (Port 443/TCP) by adding `protocol: http2` to the `config.yml`. This is much more reliable in restricted environments.

### 3. Invalid/NotFound Tunnel ID
*   **The Issue:** The initial `config.yml` contained a placeholder or mismatched Tunnel ID that gave an "Unauthorized: Tunnel not found" error.
*   **The Fix:** I had to list existing tunnels and eventually create a fresh one (`automation_tunnel_v2`) to ensure we had a valid, authorized connection linked to your specific domain.

---

## 💡 Maintenance Tips
*   **Keep `cloudflared/` safe:** This folder contains your `cert.pem` and `.json` credentials. Without these, the tunnel cannot start.
*   **Logs are your friend:** If things break, always check: `docker compose logs -f cloudflared`.
