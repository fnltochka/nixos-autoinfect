# NixOS-AutoInfect

A script to install **NixOS** on non-NixOS hosts.

This fork focuses on maximum compatibility with common Linux distributions (Debian, Ubuntu, CentOS, etc.) and supports specific cloud/virtualization platforms:

- AWS EC2/Lightsail
- Azure
- Google Compute Engine (GCE)
- OpenStack
- Docker
- LXC
- QEMU
- VMware
- VirtualBox
- Xen

If you use another platform, the script will use the default logic suitable for most bare-metal and virtual servers.

---

## What is this?

NixOS-AutoInfect is a script for installing NixOS on non-NixOS hosts. It is designed for high compatibility with common Linux distributions (Debian, Ubuntu, CentOS, etc.) and various cloud/virtualization platforms.

**Warning**: This script can render a system inoperable. Use with extreme caution and preferably only on newly provisioned systems.

---

## Usage

1. **Read and understand the [script](nixos-autoinfect)**, as it **destroys** your existing root filesystem.
2. Provision a fresh VPS, dedicated server, or cloud VM running a non-Nix distro (Debian, Ubuntu, CentOS, etc.).
3. After installation, SSH login as root with password is enabled (the password is shown by the script before reboot). For better security, it is recommended to also add an SSH key for root and/or disable password login after your first login.
4. Run something like:

   ```bash
   curl https://raw.githubusercontent.com/fnltochka/nixos-autoinfect/main/nixos-autoinfect \
     | NIX_CHANNEL=nixos-25.05 NIXOS_STATE_VERSION=25.05 bash -x
   ```

   You can specify a different `NIX_CHANNEL`, e.g. `nixos-25.05`.
   You can also set `NIXOS_STATE_VERSION` to control the generated `system.stateVersion` (defaults to `25.05`).
   See [system.stateVersion documentation](https://search.nixos.org/options?channel=unstable&show=system.stateVersion&from=0&size=50&sort=relevance&type=packages&query=system.stateVersion) for details.

At the end of the process, your system reboots into NixOS.  
The script prints out the randomly generated **root password** before reboot, if none was previously set.  
**Remember** to store it safely or change it immediately.

**Note:** After installation, SSH login as root with password is enabled by default (`PermitRootLogin yes`, `PasswordAuthentication true`).
For security, it is strongly recommended to change the root password and/or disable password login after your first login.

---

## Supported platforms

This script attempts to **auto-detect** the cloud/virtualization platform. If a specific platform is detected, NixOS will be configured with the appropriate modules. If auto-detection is not successful, it will fall back to a **generic** configuration suitable for most environments.

You can **explicitly set** the provider by using the `PROVIDER` environment variable, for example: `PROVIDER=aws`.

**Currently recognized providers for auto-detection (and explicit setting):**

- `aws` (for AWS EC2/Lightsail)
- `azure`
- `gce` (Google Compute Engine)
- `openstack`
- `docker`
- `lxc`
- `qemu` (for KVM/QEMU guests)
- `vmware`
- `virtualbox`
- `xen` (for Xen DomU)
- `hetznercloud` (via specific file check)
- `generic` (fallback for unrecognized platforms or when explicitly set for a general approach)

---

## Tested platforms

This script has been tested on:

- Ubuntu 24.04 LTS
- Ubuntu 22.04 LTS
- Ubuntu 20.04 LTS
- Debian 12 (Bookworm)

If you use another platform, the script will use the default logic suitable for most bare-metal and virtual servers.

---

## Disclaimer

Use at your own risk!  
This script removes and replaces your OS with NixOS. Make sure you have backups or that your server is easily re-deployable.

If you encounter issues, please open an issue or pull request.

---

**Thanks to the original authors of [`nixos-infect`](https://github.com/elitak/nixos-infect)** and all contributors.  
This fork aims to provide just a few tweaks while preserving compatibility with the original approach.
