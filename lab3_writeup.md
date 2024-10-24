
# CIS6325F24 Lab 3: Network Communication and Firewall Management

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
   ifconfig
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

### Step 2: Set Up the Apache Server on Kali
1. **Start the Apache web server** on the Kali VM:
   ```
   K-Menu → Services Menu → HTTPD Menu → apache start
   ```

2. On the **Ubuntu VM**, open Firefox and **request the index.html page** from the Kali server:
   ```bash
   firefox http://[Kali IP]/index.html
   ```

---

### Step 3: Capture Traffic Using Wireshark
1. **Launch Wireshark** on the Kali VM:
   ```bash
   wireshark
   ```

2. Apply the following **filter** to capture HTTP traffic:
   ```plaintext
   tcp.port == 80 or http
   ```

3. Observe the **TCP handshake**:
   - **SYN, SYN-ACK, ACK** packets between the Ubuntu VM and Kali server indicate that the connection was successfully established.

4. **Analyze the HTTP GET request and response**:
   - **GET /index.html**: Ubuntu VM requests the `index.html` page.
   - **200 OK**: Kali server responds, delivering the requested web page.

---

### Step 4: Configure UFW Firewall on Kali VM
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

---

### Step 5: Block Traffic from Ubuntu Using UFW
1. **Block HTTP traffic** from the Ubuntu VM:
   ```bash
   sudo ufw deny from [Ubuntu IP] to any port http
   ```

2. **Verify that the HTTP connection is blocked**:
   - Try accessing the Apache server from the Ubuntu VM’s Firefox again. The page should **fail to load**.

3. **Delete the deny rule**:
   ```bash
   sudo ufw delete deny from [Ubuntu IP] to any port http
   ```

4. **Insert a deny rule** at a specific position:
   ```bash
   sudo ufw insert 1 deny from [Ubuntu IP] to any port http
   ```

5. **List UFW rules with numbers** to confirm the insertion:
   ```bash
   sudo ufw status numbered
   ```

6. **Reset UFW** (remove all rules and disable the firewall):
   ```bash
   sudo ufw reset
   ```

---

### Step 6: Use Nmap to Scan the Kali Server
1. **Run an Nmap scan** from the Ubuntu VM to identify open ports on the Kali VM:
   ```bash
   nmap [Kali IP]
   ```

2. **Capture the scan results** with Wireshark and observe the following:
   - **Open or closed ports** based on TCP RST/ACK responses.
   - **Impact of UFW rules** on the scan results (e.g., blocked vs. allowed services).

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
- **Firewall rules** using UFW were effective in blocking and allowing traffic selectively, such as denying HTTP traffic from the Ubuntu VM.
- **Wireshark** provided valuable insights into network communication by capturing TCP handshakes, HTTP requests, and Nmap scan results.
- **Nmap** showed how open and closed ports could be identified and how firewall rules impact scan results.
- **Resetting UFW** cleared all configurations, demonstrating the importance of managing rules carefully to maintain security.

---

## Conclusion:
This lab provided hands-on experience with **network communication, firewall management, web server administration**, and **traffic analysis** using tools like **Apache, UFW, Wireshark, and Nmap**. The exercise emphasized the importance of **firewall configurations** in controlling access to services and highlighted how **network monitoring tools** can be used to gain visibility into network activities and potential vulnerabilities.
