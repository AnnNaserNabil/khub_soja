# Automation Setup: n8n, Portainer, & Cloudflare Tunnel

This directory contains a Docker Compose setup for running n8n and Portainer behind a **Locally Managed** Cloudflare Tunnel.

## 📁 Key Documentation

*   **[USERS_GUIDE.md](file:///mnt/data/wd/Automation/USERS_GUIDE.md)**: 🏁 Start here! Instructions on how to boot up, access, and use the services.
*   **[tunnel_recovery_guide.md](file:///mnt/data/wd/Automation/tunnel_recovery_guide.md)**: 🛠️ Guide for fixing or re-setting up the tunnel if it breaks.

## 🚀 Quick Setup Overview

1.  **Environment Variables**: Ensure `.env` is configured with your `N8N_ENCRYPTION_KEY` and domains.
2.  **Tunnel Config**: Local configuration is stored in the `cloudflared/` directory.
3.  **Launch**:
    ```bash
    docker compose up -d
    ```

## 🔐 Persistence
*   **n8n data**: Stored in `./n8n_data`.
*   **Portainer data**: Stored in `./portainer_data`.
*   **Tunnel Credentials**: Stored in `./cloudflared/`.

---
*For detailed instructions on setup, permissions, and troubleshooting, please refer to the guides listed above.*
