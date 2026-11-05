# 🛠️ Wazuh setup

**Date:** 6 November 2026
**Goal:** Set up Wazuh as the SIEM for my home SOC lab, keep it isolated, and replace its default passwords.

## 🖥️ Setup

| Item | Details |
| --- | --- |
| Virtualization software | VirtualBox |
| Wazuh deployment | Pre-built OVA (all-in-one VM) |
| RAM | 6 GB |
| Network | Host-only adapter |
| Wazuh VM IP address | 192.168.56.102 |

## 📝 Steps I took

### 1. Imported the Wazuh VM
- Downloaded the OVA file from the official Wazuh documentation.
- Imported it into VirtualBox.
- Reduced the RAM to 6 GB to leave memory for my laptop and other lab VMs.

### 2. Configured the VM before starting it
- Set the graphics controller to `VMSVGA` to help prevent the VM window from freezing.
- Enabled **Hardware Clock in UTC Time** to help keep log timestamps accurate.
- Changed the network adapter from the default bridged mode to host-only, so the VM stays off my real network.

### 3. Found the IP address and opened the dashboard
- Logged in to the VM and ran `ip a` to find its IP address.
- Opened `https://192.168.56.102` in my browser.
- Accepted the browser's certificate warning, which is expected because the lab uses a self-signed certificate.
- Confirmed that the dashboard loaded.
  <img width="1917" height="1015" alt="image" src="https://github.com/user-attachments/assets/f1da0f28-c13d-4eee-8fe8-15322208ac7a" />
  <img width="1917" height="951" alt="image" src="https://github.com/user-attachments/assets/afbea49d-cf9c-49a1-a4ee-af32654aa13b" />



### 4. Replaced the default passwords
- Connected to the VM over SSH from PowerShell.
- Changed the Linux password for `wazuh-user` with `passwd`.
- Ran the Wazuh password tool to change the Wazuh indexer and API passwords:

  `sudo bash /usr/share/wazuh-indexer/plugins/opensearch-security/tools/wazuh-passwords-tool.sh -a -A -au wazuh -ap <current-password>`
- Rebooted the VM with `sudo reboot`.

## ✅ What happened

- The VM received an IP address on the host-only network.
- The dashboard opened successfully.
- The dashboard showed no registered agents because I haven't connected a target machine yet.
- I could log in to both the VM and the dashboard after changing the passwords.

## ⚠️ Problems and fixes

### Dashboard login failed after changing the passwords

**What happened:** The dashboard showed that my username or password was incorrect, even though the same password worked on the VM.

**Cause:** I entered my Linux password. The VM and the dashboard use separate accounts, and the password tool had generated a new password for the dashboard's `admin` account.

**Fix:** I logged in using the generated `admin` password from the tool's output.

## 💡 What I learned

- A SIEM collects security logs in one place and raises alerts from them.
- Host-only networking keeps lab VMs off my real network.
- Default passwords should be changed because anyone can look them up.
- Different systems can have separate accounts and passwords, so an analyst should check which account and system a failed login was for.

## 🔒 Security note

Real passwords are not stored in this repository.

## ➡️ Next step

Create an Ubuntu Server VM and connect it to Wazuh with an agent.
