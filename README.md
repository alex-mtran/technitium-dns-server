# Technitium DNS Server

Self-hosted DNS server powered by Technitium on Ubuntu Server 24.04 featuring ad blocking, custom DNS records, DoH/DoT support, and full local network control.

#### Required Software
* Ubuntu Server 24.04
* Oracle VirtualBox
* Technitium Linux

### Set up Ubuntu Desktop 24.04 Virtual Machine

1. <a href="https://ubuntu.com/download/desktop">Download Ubuntu Desktop 24.04.4 LTS</a>.
2. <a href="https://www.virtualbox.org/wiki/Downloads">Download Oracle VirtualBox</a>.
3. Open **Oracle VirtualBox** and press **New** to add a new virtual machine to the manager.
4. Fill in:

    **Virtual Name and OS**

<img width="926" height="749" alt="Virtual machine name and operating system" src="https://github.com/user-attachments/assets/bbeb0690-d03c-4617-af10-baf64aa045e7" />

    **Credentials**

<img width="924" height="746" alt="Credentials" src="https://github.com/user-attachments/assets/f1640dd3-8afd-41a2-a289-0a0f13dc7430" />

    **Virtual Hardware**

<img width="925" height="748" alt="Virtual Hardware" src="https://github.com/user-attachments/assets/0e2a468c-bf58-4bf5-8f21-eb3161212ccc" />

    **Virtual Hard Disk**

<img width="926" height="743" alt="Virtual Hard Disk" src="https://github.com/user-attachments/assets/bcaf6555-6f3d-4b4b-9e36-8edefd734947" />

5. Click **Finish**.

#### Configure Server VM Settings

1. Open **Technitium DNS** server VM and wait for initial installation to finish.
2. Update and upgrade packages using `sudo apt update & sudo apt upgrade -y`. Make sure to run this command periodically to stay up to date.
3. Run the **Technitium DNS Server** installer script via the command `curl -sSL https://download.technitium.com/dns/install.sh | sudo bash`.

<img width="1279" height="882" alt="Technitium successfully installed" src="https://github.com/user-attachments/assets/62382c25-ef6c-47cb-8078-79ff49ce8240" />

4. Type `sudo nano /etc/netplan/` and tab complete to write to the machine's config.yaml file. Run the completed command to write into the file.

<img width="1278" height="889" alt="/etc/netplan" src="https://github.com/user-attachments/assets/465e6f8b-174f-4abf-90d7-0eaeb1d35d68" />

<img width="1280" height="882" alt="Initial config.yaml file" src="https://github.com/user-attachments/assets/1e7510cc-3163-4e24-8da5-0b7e9a164a57" />

5. Change config.yaml contents to match the image below. Hit **Ctrl + X > Y > Enter** to save contents.

