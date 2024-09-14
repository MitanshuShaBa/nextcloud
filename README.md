# Nextcloud Custom Setup
At the end of 2023, I maxed out my Google storage again and decided it was time to stop juggling multiple accounts. Instead of paying for more storage, I repurposed an old laptop and built my own cloud using Nextcloud. Now, I can access my files securely from anywhere, and you can too!

Prerequisites
- An old laptop with Linux installed and sufficient storage
- A registered domain name
- A Cloudflare account

## Steps
1. Install Docker
Ensure Docker is installed on your Linux system.

```bash
# Update package database
sudo apt update

# Install Docker
sudo apt install docker.io -y

# Start Docker service
sudo systemctl start docker

# Enable Docker to start on boot
sudo systemctl enable docker

# Verify Docker installation
docker --version
```

2. Find Your Internal IP Address
You'll need the internal IP to create the Cloudflare tunnel.
Note the IP address from the output, typically associated with eth0 or wlan0.

```bash
ip a
```

3. Set Up Cloudflare Tunnel

    1. Go to Cloudfare Dashboard
    2. Click on Zero Trust
    3. In Networks, go to Tunnels
    4. Create a tunnel which connects to `http://<IP>:11000`
    5. Copy the token for next step

4. Prepare Docker Compose File

    1. Copy the compose.yml file from this repo
    2. Replace values of cloudfare token, IP and storage locations in the file

5. Run Docker Compose

```bash
docker-compose up -d
```

6. Access the Nextcloud Instance
Once the master container is running, visit the following URL in your browser to finish the setup:

```bash
https://<internal-ip>:8080
```
7. Create an admin password and save your admin credentials.
7. Add your domain to the settings for external access.
7. Configure the backup location using nextcloud_aio_backupdir.
7. Enable Optional Containers
In the Nextcloud All-in-One settings, enable all optional containers to extend functionality (e.g., Clam AV, Collabora etc.).

8. Access via Domain
After setup, your Nextcloud instance will be available at `https://your-domain.com` through the Cloudflare tunnel.

Backup and Restore
Make sure your backups are configured properly in the nextcloud_aio_backupdir volume. You can easily restore from these backups if needed. It is recommended to turn on daily backups and automatic container updates

