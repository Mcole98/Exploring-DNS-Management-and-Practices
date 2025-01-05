<p align="center">
<img src="https://i.imgur.com/fTIAX6A.png" alt="dns lab pic"/>
</p>

# Microsoft Azure DNS Lab Tutorial

## Introduction
This lab focuses on DNS configuration and testing using A-Records, Local DNS Cache, and CNAME Records. We will be utilizing the **DC-1** and **Client-1** virtual machines (VMs) that were set up in the previous Active Directory lab. Ensure that these VMs are properly configured and running before proceeding with the exercises.

---

## Prerequisites
- Access to **DC-1** and **Client-1** VMs.
- Administrator credentials for the domain: `mydomain.com\jane_admin` (from the AD lab).
- Working knowledge of DNS records.

---

## A-Record Exercise

1. **Connect to DC-1**:
    - Log into **DC-1** using the domain admin account: `mydomain.com\jane_admin`.

2. **Connect to Client-1**:
    - Log into **Client-1** as an admin: `mydomain\jane_admin`.

3. **Verify that "mainframe" is Unreachable**:
    - On **Client-1**, open the Command Prompt and run:
      ```bash
      ping mainframe
      ```
      Observe that the ping fails.

<p>
<img src="https://i.imgur.com/V9aQCZI.png" height="80%" width="80%" alt="Lab 6"/>
</p>

Run the following command to check for the DNS record: 'nslookup mainframe'
Observe that it fails (no DNS record exists).

<p>
<img src="https://i.imgur.com/1Jo2maG.png" height="80%" width="80%" alt="Lab 6"/>
</p>

4. **Create a DNS A-Record**:
    - Switch to **DC-1**.
    - Open the **DNS Manager**.
    - Navigate to the appropriate forward lookup zone.
    - Add an **A-Record** for `mainframe` and point it to **DC-1's Private IP address**.
  
<p>
<img src="https://i.imgur.com/MyHo9ld.png" height="80%" width="80%" alt="Lab 6"/>
</p>

5. **Verify Connectivity to "mainframe"**:
    - Go back to **Client-1**.
    - Run:
      ```bash
      ping mainframe
      ```
      Observe that the ping is now successful.

<p>
<img src="https://i.imgur.com/VlbzsZN.png" height="80%" width="80%" alt="Lab 6"/>
</p>

---

## Local DNS Cache Exercise

1. **Modify the A-Record**:
    - On **DC-1**, change the `mainframe` A-Record to point to `8.8.8.8`.
  
<p>
<img src="https://i.imgur.com/2wUg8UM.png" height="80%" width="80%" alt="Lab 6"/>
</p>

2. **Ping "mainframe" from Client-1**:
    - On **Client-1**, run:
      ```bash
      ping mainframe
      ```
      Observe that the ping still resolves to the old address (cached locally).

3. **Check the Local DNS Cache**:
    - On **Client-1**, display the DNS cache using:
      ```bash
      ipconfig /displaydns
      ```
      Observe the cached record for `mainframe`.

<p>
<img src="https://i.imgur.com/ri19Yl6.png" height="80%" width="80%" alt="Lab 6"/>
</p>

4. **Flush the DNS Cache**:
    - Clear the local DNS cache using:
      ```bash
      ipconfig /flushdns
      ```

5. **Verify Cache Clearing**:
    - Confirm the cache is empty by running:
      ```bash
      ipconfig /displaydns
      ```

6. **Verify the Updated A-Record**:
    - Attempt to ping `mainframe` again:
      ```bash
      ping mainframe
      ```
      Observe that it now resolves to the updated address (`8.8.8.8`).

<p>
<img src="https://i.imgur.com/lf9550j.png" height="80%" width="80%" alt="Lab 6"/>
</p>

---

## CNAME Record Exercise

1. **Create a CNAME Record**:
    - On **DC-1**, open the **DNS Manager**.
    - Navigate to the appropriate forward lookup zone.
    - Add a **CNAME Record**:
      - Alias: `search`.
      - Points to: `www.google.com`.
     
<p>
<img src="https://i.imgur.com/CDzfQAm.png" height="80%" width="80%" alt="Lab 6"/>
</p>

2. **Test the CNAME Record**:
    - On **Client-1**, ping `search`:
      ```bash
      ping search
      ```
      Observe the results of the CNAME record resolution.

3. **Verify Using nslookup**:
    - On **Client-1**, run:
      ```bash
      nslookup search
      ```
      Observe the results, ensuring the alias resolves to `www.google.com`.

<p>
<img src="https://i.imgur.com/ufpwziw.png" height="80%" width="80%" alt="Lab 6"/>
</p>

---

## Conclusion
Congratulations! You have successfully completed the DNS exercises for A-Records, Local DNS Cache, and CNAME Records.

---
