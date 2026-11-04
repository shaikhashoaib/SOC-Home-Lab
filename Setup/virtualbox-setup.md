# VirtualBox and Ubuntu Server Setup

## Project status

- [x] Created the `SOC-Ubuntu` virtual machine in Oracle VirtualBox.
- [x] Attached the Ubuntu Server 24.04.5 LTS AMD64 installer ISO.
- [x] Completed the Ubuntu Server installation.
- [ ] Verify the first boot and successful console login.
- [ ] Configure the lab network and record the VM IP address.

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
| Guest OS profile | Ubuntu 64-bit (VirtualBox detected profile) |
| Installer media | Ubuntu Server 24.04.5 LTS AMD64 |
| Memory | 2048 MB |
| Virtual CPUs | 1 |
| Virtual disk | 40.48 GB |
| Network adapter | NAT |
| EFI | Disabled |
| Unattended installation | Disabled; manual installation used |

> Note: VirtualBox displayed an Ubuntu 24.10 guest-profile label during creation, but the attached installer ISO was Ubuntu Server 24.04.5 LTS. The installer media determines the Ubuntu release installed.

## Installation notes

1. Created the VM under the `E:\\SOC-Lab\\VMs` folder.
2. Attached the Ubuntu Server 24.04.5 LTS AMD64 ISO from `E:\\SOC-Lab\\ISO`.
3. Used manual installation rather than VirtualBox unattended installation.
4. Left the HTTP proxy field blank.
5. Created a local administrative user during Ubuntu setup. Credentials are intentionally not documented here.
6. Ubuntu reported **Installation complete** and the VM was rebooted.
7. First-boot login verification is still pending.

## Resource considerations

The host has 8 GB RAM and a 2-core processor. The VM is intentionally configured with 2 GB RAM and 1 virtual CPU to leave resources for Windows. A full Wazuh all-in-one deployment may be too demanding on this host; resource usage will be monitored and the architecture adjusted if necessary.

## Next steps

- Confirm Ubuntu boots from its virtual disk and log in successfully.
- Update Ubuntu packages.
- Verify network connectivity and record the VM's private lab IP address.
- Decide on a suitable lightweight Wazuh deployment before installing the SIEM.

## Evidence

Screenshots will be added here after they are captured and reviewed. Sensitive information such as passwords, tokens, public IP addresses, and unredacted logs must not be committed.
