# VirtualBox and Ubuntu Server Setup

## Project status

- [x] Created the `SOC-Ubuntu` virtual machine in Oracle VirtualBox.
- [x] Installed Ubuntu Server 24.04.5 LTS AMD64.
- [x] Verified Ubuntu boot and login.
- [x] Updated Ubuntu packages.
- [x] Configured SSH remote access from Windows.
- [x] Configured NAT port forwarding for SSH.
- [x] Added a Host-only adapter for Windows/Ubuntu lab communication.
- [x] Verified Windows-to-Ubuntu connectivity over the Host-only network.

## Host computer

| Component | Specification |
|---|---|
| Operating system | Windows 10 Pro 64-bit |
| Computer | HP ProDesk 400 G1 SFF |
| Processor | Intel Core i3-4160 @ 3.60 GHz |
| CPU | 2 physical cores / 4 logical processors |
| Memory | 8 GB RAM |
| Virtualization | Enabled in BIOS |
| Virtualization software | Oracle VirtualBox |

## SOC-Ubuntu virtual machine

| Setting | Configuration |
|---|---|
| VM name | `SOC-Ubuntu` |
| Installer media | Ubuntu Server 24.04.5 LTS AMD64 |
| Memory | 2048 MB |
| Virtual CPUs | 1 |
| Virtual disk | 40.48 GB |
| Adapter 1 | NAT |
| Adapter 2 | Host-only Adapter |
| Guest NAT address observed | `10.0.2.15` |
| Guest Host-only address configured | `192.168.56.10/24` |
| Windows Host-only address observed | `192.168.56.1/24` |
| SSH forwarding | Host TCP `2222` to guest TCP `22` |

## Installation and access notes

1. Created the VM under `E:\SOC-Lab\VMs\SOC-Ubuntu`.
2. Attached the Ubuntu Server 24.04.5 LTS AMD64 ISO from `E:\SOC-Lab\ISO`.
3. Used manual installation.
4. Created a local administrative account. Credentials are intentionally not documented.
5. Installed and enabled `openssh-server`.
6. Configured VirtualBox NAT port forwarding so Windows can connect with:

   ```powershell
   ssh -p 2222 socadmin@127.0.0.1
   ```

7. Added Adapter 2 as a Host-only Adapter while keeping Adapter 1 as NAT.
8. The guest Host-only interface was brought up and assigned `192.168.56.10/24` for this lab session.
9. Verified connectivity from Ubuntu to the Windows Host-only address:

   ```bash
   ping -c 4 192.168.56.1
   ```

   The captured test returned 4 packets received and 0% packet loss.

10. Verified the Splunk receiving port from Ubuntu:

   ```bash
   timeout 3 bash -c 'cat < /dev/null > /dev/tcp/192.168.56.1/9997' && echo CONNECTED || echo FAILED
   ```

   The test returned `CONNECTED` after a Windows Firewall inbound rule was created for TCP 9997 from the Ubuntu Host-only IP.

> **Address persistence:** The Ubuntu Host-only IPv4 address was assigned manually with `ip addr add`; this may not persist after reboot. Configure persistent addressing only after confirming the VirtualBox Host-only network settings.

## Resource considerations

The host has 8 GB RAM and a 2-core processor. The Ubuntu VM is intentionally configured with 2 GB RAM and 1 virtual CPU to leave resources for Windows. A full Wazuh all-in-one deployment may be too demanding on this host, so Splunk Enterprise on Windows is the current primary SIEM.

## Next steps

- Install Splunk Universal Forwarder on Ubuntu.
- Configure Ubuntu authentication/system log forwarding to Splunk at `192.168.56.1:9997`.
- Verify Linux events in Splunk before marking forwarding complete.

## Evidence

Two image files are currently present in the repository's `Screenshots/` folder. Their filenames are generic; add clear captions or rename them after confirming their contents. Never commit passwords, tokens, public IP addresses, or unredacted personal log data.
