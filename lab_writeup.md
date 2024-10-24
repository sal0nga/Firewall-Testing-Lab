
# Firewall Testing Lab: Network Communication and Firewall Management

### Objective:
The goal of this lab is to explore **network communication** between two virtual machines (Kali and Ubuntu), set up and manage an **Apache web server**, configure **firewall rules with UFW**, analyze **traffic using Wireshark**, and observe how firewall configurations impact network access.

---

## Lab Environment:
- **VM 1**: Ubuntu (acting as a client)
- **VM 2**: Kali Linux (acting as a server)
- **Network Configuration**: Both VMs are configured with **bridged networking** on the same subnet.

---

## Lab Procedure:

### Step 1: Verify Network Connectivity
1. **Find the IP address** of each VM:
   ```bash
   ip a
   ```

2. **Ping the Kali VM** from the Ubuntu VM to confirm connectivity:
   ```bash
   ping [Kali IP]
   ```

3. **Stop the ping request** after confirming connectivity:
   ```bash
   Ctrl + C
   ```

---

### Step 2: Configure UFW Firewall on Kali VM
1. **Check the UFW status**:
   ```bash
   sudo ufw status
   ```

2. **Enable UFW**:
   ```bash
   sudo ufw enable
   ```

3. **Allow essential services** (FTP, SSH, HTTP):
   ```bash
   sudo ufw allow 21
   sudo ufw allow 22
   sudo ufw allow http
   ```

4. **View UFW rules with verbose output**:
   ```bash
   sudo ufw status verbose
   ```

5. **View firewall rules in IPTables**:
   ```bash
   sudo iptables -L
   ```

---

### Step 2: Use Nmap to Scan the Kali VM
1. **Run an Nmap scan** from the Ubuntu VM to identify unfiltered ports on the Kali VM:
   ```bash
   nmap [Kali IP]
   ```

2. **Capture the scan results** with Wireshark and observe the following:
   - On **Ports 21, 22 and 80**, the Kali VM returns RST/ACK responses to the Ubuntu VM's SYN packets.
   - The Nmap scan results show that those ports are **closed**.
   - The UFW rules allow traffic on those ports, but if the **underlying service is not configured**, then the ports will **remain closed** and any connection attempts will be **rejected**.


---

### Step 3: Set Up the Apache Server and Capture Traffic Using Wireshark
1. **Start the Apache web server** on the Kali VM:
   ```bash
   sudo systemctl start apache2
   sudo systemctl status apache2
   ```

2. From the Ubuntu VM, **run another Nmap scan**:
   - Since the Apache Web Server has been started, **port 80 is now open**.

3. **Launch Wireshark** on the Kali VM and Start Packet Capture:
   ```bash
   wireshark
   ```
   
4. On the **Ubuntu VM**, enable the root user to modify display,then **request the index.html page** from the Kali server:
   ```bash
   xhost +SI:localuser:root
   sudo su
   firefox http://[Kali IP]/index.html
   ```

5. Apply the following **filter** to capture HTTP traffic:
   ```plaintext
   tcp.port == 80 or http
   ```

6. Observe the **TCP handshake**:
   - **SYN, SYN-ACK, ACK** packets between the Ubuntu VM and Kali server indicate that the connection was successfully established.

7. **Analyze the HTTP GET request and response**:
   - **GET /index.html**: Ubuntu VM requests the `index.html` page.
   - **200 OK**: Kali server responds, delivering the requested web page.

---

### Step 5: Block Traffic from Ubuntu Using UFW
1. **Block HTTP traffic** from the Ubuntu VM:
   ```bash
   sudo ufw deny from [Ubuntu IP] to any port http
   ```

2. **Verify that the HTTP connection is NOT blocked**:
   - Access the Apache server from the Ubuntu VM’s Firefox again. The page **should not fail to load**.
   - The **order** of firewall rules matters. They are processed from **top to bottom**; the **first** rule that matches the packet will be applied. Once a match is found, the firewall **stops processing further rules**.

3. **Delete the deny rule**:
   ```bash
   sudo ufw delete deny from [Ubuntu IP] to any port http
   ```

4. **Reinsert the deny rule** at the **first** position:
   ```bash
   sudo ufw insert 1 deny from [Ubuntu IP] to any port http
   ```

5. **List UFW rules with numbers** to confirm the insertion:
   ```bash
   sudo ufw status numbered
   ```
   
6. **Verify that the HTTP connection IS blocked**:
   - Access the Apache server from the Ubuntu VM’s Firefox again. The page should **fail to load**.

7. **Reset UFW** (remove all rules and disable the firewall):
   ```bash
   sudo ufw reset
   ```

---

### Step 7: Stop the Apache Server
1. **Stop the Apache web server** on the Kali VM:
   ```bash
   sudo systemctl stop apache2
   ```

---

### Step 8: Shut Down the Kali VM
1. **Shutdown the Kali VM**:
   ```bash
   shutdown -h now
   ```

---

## Observations and Takeaways:
- **TCP handshake and HTTP traffic** between the two VMs functioned correctly when the firewall allowed traffic.
- **Firewall rules** and **apropriate ordering** using UFW were effective in blocking and allowing traffic selectively, such as denying HTTP traffic from the Ubuntu VM.
- **Wireshark** provided valuable insights into network communication by capturing TCP handshakes, HTTP requests, and Nmap scan results.
- **Nmap** showed how open and closed ports could be identified and how firewall rules impact scan results.
- **Resetting UFW** cleared all configurations, demonstrating the importance of managing rules carefully to maintain security.

---

## Conclusion:
This lab provided hands-on experience with **network communication, firewall management, web server administration**, and **traffic analysis** using tools like **Apache, UFW, Wireshark, and Nmap**. The exercise emphasized the importance of **firewall configurations** in controlling access to services and highlighted how **network monitoring tools** can be used to gain visibility into network activities and potential vulnerabilities.
