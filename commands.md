# Commands
The list of commands used in this lab.

## Configuring UFW
- sudo ufw status
- sudo ufw enable
- sudo ufw allow 21
- sudo ufw allow http
- sudo ufw status verbose
- sudo ufw deny from [Ubuntu IP] to any port http
- **List UFW rules with numbers:** sudo ufw status numbered
- sudo ufw delete deny from [Ubuntu IP] to any port http
- sudo ufw insert 1 deny from [Ubuntu IP] to any port http
- sudo ufw reset

## Running the Apache Web Server on Kali
sudo systemctl start apache2

sudo systemctl status apache2

sudo systemctl stop apache2

## Requesting Web Service on Ubuntu

### Use xhost to allow root access to display:
xhost +SI:localuser:root

### To switch to root user:
sudo su

### To request a web page from the Apache server:
firefox http://[Kali IP]/index.html
