UFW Commands

sudo ufw staus
sudo ufw enable

sudo ufw allow 21
sudo ufw allow http

sudo ufw status verbose

sudo ufw deny from [Ubuntu IP] to any port http

sudo ufw status numbered

sudo ufw delete deny from [Ubuntu IP] to any port http
sudo ufw insert 1 deny from [Ubuntu IP] to any port http

sudo ufw reset
