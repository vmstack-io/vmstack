# VMStack

**Self-hosted VM provisioning portal for Proxmox VE.** Students register, get approved by a teacher, and launch their own lab VMs from templates — no Proxmox access needed.

- Student self-registration with teacher approval
- Template-based VM provisioning with per-node limits
- Runs as Docker containers next to your Proxmox cluster

**Free edition:** 1 Proxmox node, up to 3 users. Need more nodes or users? See [vmstack.io](https://vmstack.io) for a Pro license.

## Quick start

Requirements: x86-64 Linux host with Docker Engine 24+ (Compose plugin), network access to Proxmox VE 8.x/9.x on port 8006.

```bash
sudo mkdir -p /opt/vmstack && cd /opt/vmstack
sudo curl -fsSLO https://raw.githubusercontent.com/vmstack-io/vmstack/main/docker-compose.yml
sudo curl -fsSL -o .env https://raw.githubusercontent.com/vmstack-io/vmstack/main/.env.example
sudo nano .env          # set VMSTACK_URL, VMSTACK_SECRET (openssl rand -hex 32), VMSTACK_ADMIN_EMAIL
sudo docker compose up -d
```

Then open `http://<host-ip>:8080`, create the admin account and add your Proxmox node.

## Proxmox API token

Run on your Proxmox node as root:

```bash
pveum user add vmstack@pve --comment "VMStack portal"
pveum role add VMStackRole -privs "VM.Allocate VM.Clone VM.Audit VM.PowerMgmt VM.Console VM.Config.Disk VM.Config.CPU VM.Config.Memory VM.Config.Network VM.Config.Options VM.Config.Cloudinit VM.Config.CDROM Datastore.AllocateSpace Datastore.Audit SDN.Use Sys.Audit Pool.Audit"
pveum acl modify / --users vmstack@pve --roles VMStackRole
pveum user token add vmstack@pve vmstack --privsep 0
```

Copy the token secret — Proxmox shows it only once. In VMStack, add the node with token ID `vmstack@pve!vmstack` and that secret.

## Update

```bash
cd /opt/vmstack
sudo docker compose pull && sudo docker compose up -d
```

## Support

Community support via [GitHub Discussions](https://github.com/vmstack-io/vmstack/discussions). Bugs: [Issues](https://github.com/vmstack-io/vmstack/issues).

---

VMStack is developed by [BlueSense](https://vmstack.io), Aalst, Belgium. Free to use within the limits of the Free edition; see LICENSE.
