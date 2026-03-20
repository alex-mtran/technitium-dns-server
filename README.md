# Technitium DNS Server

Self-hosted DNS server powered by Technitium on Ubuntu Desktop 24.04 featuring ad blocking, custom DNS records, DoH/DoT support, and full local network control.

#### Required Software
* Ubuntu Desktop 24.04 LTS
* Oracle VirtualBox
* Technitium Linux

### Set up Ubuntu Desktop 24.04.4 Virtual Machine

1. <a href="https://ubuntu.com/download/desktop">Download Ubuntu Desktop 24.04.4 LTS</a>.
2. <a href="https://www.virtualbox.org/wiki/Downloads">Download Oracle VirtualBox</a>.
3. Open **Oracle VirtualBox** and press **New** to add a new virtual machine to the manager.
4. Fill in:

   **Virtual Name and OS**

<img width="924" height="746" alt="Virtual machine name and operating system" src="https://github.com/user-attachments/assets/89f84ed8-38ce-46ee-82f1-82bc7fed703a" />

   **Credentials**

<img width="924" height="746" alt="Credentials" src="https://github.com/user-attachments/assets/f1640dd3-8afd-41a2-a289-0a0f13dc7430" />

   **Virtual Hardware**

<img width="925" height="748" alt="Virtual Hardware" src="https://github.com/user-attachments/assets/0e2a468c-bf58-4bf5-8f21-eb3161212ccc" />

   **Virtual Hard Disk**

<img width="922" height="743" alt="Virtual hard disk" src="https://github.com/user-attachments/assets/d2bc88d8-b391-4a30-ade9-0b9617abbf3f" />

5. Click **Finish**.

#### Install Technitium

1. Open **TechnitiumDNS** VM and wait for initial installation to finish.
2. Open up the terminal by pressing **Ctrl + Alt + T**. Update and upgrade packages using `sudo apt update & sudo apt upgrade -y`. Make sure to run this command periodically to stay up to date.

<img width="1282" height="882" alt="Update and upgrade packages" src="https://github.com/user-attachments/assets/adac03a9-60ba-4f95-b747-bfb5b7b050a1" />

3. Run the **Technitium DNS Server** installer script via the command `curl -sSL https://download.technitium.com/dns/install.sh | sudo bash`.

<img width="1274" height="882" alt="Technitium successfully installed" src="https://github.com/user-attachments/assets/bf728f8b-b768-45a9-a48d-1c51a11d3dd8" />

> **Note:** `curl` is needed to install using the above command. Install **curl** via `sudo apt install curl`.

4. Open up **Mozilla Firefox** and search for the Technitium web console by searching up the domain **localhost:5380**. Enter new password and sign in.

<img width="1275" height="882" alt="Change technitium web console password" src="https://github.com/user-attachments/assets/d88c9c87-2577-4be6-8489-a820230e4d7b" />

5. Run `sudo nano /etc/netplan/99-dns-override.yaml` command and write into the file to override the dns for this machine. Hit **Ctrl + X > Y > Enter** to save file contents and exit.

<img width="1278" height="885" alt="99-dns-override.yaml file contents" src="https://github.com/user-attachments/assets/340c25dc-24f9-4013-af4e-5173d3baf0d5" />

> **Note:** Here it is industry standard to create a separate override file to follow the principle of not modifying files that we don't own. Thus we create a new file to override the DNS rather than modifying the 01 network manager file or the 50 cloud-init file.

6. Run `sudo netplan apply` to apply netplan changes.

<img width="1276" height="881" alt="sudo netplan apply warnings" src="https://github.com/user-attachments/assets/68580af2-57be-49ac-9df3-ed1d437eb8ef" />

> **Note:** The following are commands to fix cosmetic warnings:
>
> `sudo chmod 600 /etc/netplan/99-dns-override.yaml` and `sudo chmod 600 /etc/netplan/01-network-manager-all.yaml` and then `sudo netplan apply` again to change permission warnings.
>
> `sudo nano /etc/hosts` > Add in this line **127.0.0.1 TechnitiumDNS** to resolve the hostname **TechnitiumDNS** locally. This also means you can search **technitiumdns:5380** as well as **localhost:5380** to access the technitium web console.
>
> <img width="1189" height="406" alt="Apply cosmetic warning fixes" src="https://github.com/user-attachments/assets/b46d9e49-70f5-4b4e-9768-67235c86e993" />
>
> <img width="1276" height="880" alt="/etc/hosts" src="https://github.com/user-attachments/assets/6cac7742-cd1d-499b-a8d1-b04adc39d04e" />
>
> **Note:** Verify changes with `nslookup google.com` and `ping TechnitiumDNS`. `nslookup google.com` should yield a nameserver of `127.0.0.1` and `ping TechnitiumDNS` should receive pings properly. Alternatively similar to `nslookup google.com`, run command `cat /etc/resolv.conf` to verify a nameserver of `127.0.0.1`. `127.0.0.1` is the universal IP of the local machine itself.
> 
> <img width="1275" height="881" alt="Verify changes" src="https://github.com/user-attachments/assets/81818fcf-6a91-404e-a727-2a5460461183" />
>
> <img width="1277" height="879" alt="Verify machine nameserver" src="https://github.com/user-attachments/assets/1acec34b-7e36-452b-885e-41a2d427a880" />

#### Configure Technitium

1. Open up Technitium web console via **Mozilla Firefox** and search up either **localhost:5380** or **technitiumdns:5380**. Then, click the **Settings** tab in the top bar.

<img width="1279" height="879" alt="Settings tab of technitium web console" src="https://github.com/user-attachments/assets/870bf4b8-9dd5-4b49-b3ad-bb38ced4de43" />

2. Scroll down and change the **Default Record TTL** to 600 seconds. Scroll down and click **Save Settings**.

> **Note:** DNS TTL (time to live) is a setting that tells the DNS resolver how long to cache a query before requesting a new one. To learn more read <a href="https://www.varonis.com/blog/dns-ttl">this blogpost on dns ttl</a>. We set the TTL to a relatively short 600 seconds (10 minutes) to account for changing IPs of ad websites.

3. Click the **Settings** tab > **Blocking** and scroll down to **Allow/Block List URLs**. Here you can add custom lists or use the **Quick Add** feature underneath to quickly add updated lists. My setup will use the **Steven Black [adware + malware]**, **OISD Big [Domains (Wildcards)]**, and **Hagezi [Multi PRO++-Maximum protection]**, however, further research should be done to determine the best blocklists to use for your use case.

<img width="1278" height="882" alt="Add block lists" src="https://github.com/user-attachments/assets/3559c67f-61a3-4f4e-a129-42c170e83596" />

> **Note:** Different block lists may have redundant entries. Adding more entries will lower the performance of the ad-blocking DNS server due to increased memory usage, slowed query processing, and increased maintenance to whitelist broken sites.
