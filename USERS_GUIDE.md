# 🚀 Project User Guide: n8n & Portainer with Cloudflare Tunnel

This guide explains how to start, manage, and use your automation setup.

## 🏁 Quick Start: Booting Up

To start the entire project for the first time or after a manual stop, run this command in your terminal:

```bash
docker compose up -d
```

*   **`-d` (Detached mode):** This runs the services in the background so you can close your terminal and they will keep running.

## 🌐 Accessing Your Services

Once started, you can access your tools through these secure URLs:

| Service | Public URL | Description |
| :--- | :--- | :--- |
| **n8n** | [https://n8n.mahathasan.com](https://n8n.mahathasan.com) | Your automation workflow engine. |
| **Portainer** | [https://portainer.mahathasan.com](https://portainer.mahathasan.com) | Web UI for managing your Docker containers. |

---

## 🔧 Useful Commands

### Check if everything is running
```bash
docker compose ps
```

### View real-time logs (helpful for troubleshooting)
```bash
docker compose logs -f
```

### Shut down the project
If you want to stop all services completely:
```bash
docker compose down
```

### Update services
If you change the `.env` or `docker-compose.yml` and want to apply changes:
```bash
docker compose up -d --force-recreate
```

---

## ⚡ What happens if I restart my PC?

**Everything happens automatically.**
*   The Docker service is set to start with your computer.
*   The containers are configured with `restart: unless-stopped`.
*   **Result:** As long as you didn't manually run `docker compose down` before shutting down, your n8n and Portainer will be back online as soon as your computer finishes booting up. No manual commands needed!

---

## 📁 Project Structure
*   **`docker-compose.yml`**: The "master plan" for your services.
*   **`.env`**: Stores your passwords and domain settings.
*   **`cloudflared/`**: Configuration for your secure tunnel.
*   **`n8n_data/`**: Your actual workflows and database (keep this safe!).
*   **`portainer_data/`**: Portainer's configuration data.

---

## 🆘 Troubleshooting
If the URLs are not loading:
1. Check the tunnel logs: `docker compose logs cloudflared`
2. Check the recovery guide: [tunnel_recovery_guide.md](file:///mnt/data/wd/Automation/tunnel_recovery_guide.md)
