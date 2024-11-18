##### Module 4: Remote Access Setup - Part 2

**Why Move to the Cloud?**
Moving your FreshRSS instance to the cloud provides several benefits:
- Access your feeds from anywhere without keeping your local machine running
- Consistent feed updates even when your computer is off
- Better reliability with professional hosting infrastructure
- Easier mobile access without VPN or port forwarding
- Regular automated backups

**Understanding the /opt Directory**
Before we begin, it's important to understand where we'll install FreshRSS:
- /opt is the standard Linux directory for "optional" or third-party software
- Separates additional software from system files (/bin, /usr, /etc)
- Makes backups and maintenance cleaner and more organized
- Follows Linux filesystem hierarchy standards
- Commonly used for self-contained applications

**Firewall Configuration**
First, we secure the server while ensuring FreshRSS remains accessible:

```bash
# Install UFW (Uncomplicated Firewall)
sudo apt update
sudo apt install ufw

# Set default policies
sudo ufw default deny incoming    # Block all incoming traffic by default
sudo ufw default allow outgoing   # Allow all outgoing traffic by default

# Allow SSH (port 22)
sudo ufw allow ssh
```

Understanding the flags and commands:
- `sudo`: SuperUser DO - Executes command with administrative privileges
- `apt update`: Refreshes package list from repositories
- `ufw allow ssh`: Creates rule to allow SSH traffic
- `default deny/allow`: Sets baseline security policy

**Why These Firewall Rules?**
- Default deny incoming: Blocks potential attacks by limiting access
- Default allow outgoing: Lets your server fetch RSS feeds and updates
- Allow SSH: Maintains your ability to manage the server
- We'll add HTTP/HTTPS rules after Docker setup

**Docker Installation**
Since we're migrating an existing setup, we need Docker on the cloud server:

```bash
# Remove potential conflicting packages
sudo apt remove docker docker-engine docker.io containerd runc

# Install prerequisites
sudo apt install ca-certificates curl gnupg

# Add Docker's GPG key for package verification
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the Docker repository to apt sources 

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Update apt package list with the new repository 

sudo apt-get update 

# Install Docker packages 

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Understanding the flags:
- `-m 0755`: Sets directory permissions (owner can write, everyone can read/execute)
- `-d`: Creates directory if it doesn't exist
- `-fsSL`: 
  - `f`: Fail silently on errors
  - `s`: Silent mode
  - `S`: Show error if fails
  - `L`: Follow redirects

**Migrating Your FreshRSS Data**
Now we'll transfer your existing setup:

```bash
# On your local machine, create a backup of your FreshRSS data
cd ~/rss-learning-project/docker-config
tar -czf freshrss-backup.tar.gz freshrss/

# Copy to cloud server using SCP (Secure Copy)
scp -i ~/.ssh/freshrss-key freshrss-backup.tar.gz username@your-server-ip:/home/username/
```

Understanding tar flags:
- `-c`: Create archive
- `-z`: Compress with gzip
- `-f`: Specify filename

SCP flags:
- `-i`: Identity file (your SSH key)

**On the Cloud Server**
Extract and set up your migrated data:

```bash
# Create the destination directory
mkdir -p /opt/freshrss

# Set appropriate ownership
sudo chown -R $USER:$USER /opt/freshrss

# Extract backup
cd ~
tar -xzf freshrss-backup.tar.gz -C /opt/freshrss
```

Understanding tar flags:
- `-x`: Extract archive
- `-C`: Change to directory before extracting

**Docker Compose Configuration**
Create and edit the configuration file:

```bash
# Open docker-compose.yml in nvim
nvim /opt/freshrss/docker-compose.yml
```

In nvim:
1. Press `i` to enter insert mode
2. Paste the following configuration:

```yaml
services:
  freshrss:
    image: freshrss/freshrss:latest
    container_name: freshrss
    ports:
      - "80:80"
    volumes:
      - ./freshrss/data:/var/www/FreshRSS/data
      - ./freshrss/extensions:/var/www/FreshRSS/extensions
    environment:
      - TZ=Europe/London
      - CRON_MIN=*/15
      - FRESHRSS_BASE_URL=http://your-server-ip
    restart: unless-stopped
```

3. Press `Esc` to exit insert mode
4. Type `:wq` and press `Enter` to save and quit

Understanding nvim commands:
- `i`: Enter insert mode for typing/pasting
- `Esc`: Return to command mode
- `:w`: Write (save) the file
- `:q`: Quit nvim
- `:wq`: Combine write and quit
- `:q!`: Quit without saving (if you make a mistake)

**Why These Docker Settings?**
- `ports: "80:80"`: Makes FreshRSS accessible via standard HTTP port
- `volumes`: Maintains your existing configuration and data
- `CRON_MIN=*/15`: Updates feeds every 15 minutes
- `restart: unless-stopped`: Ensures service stays running

**Final Firewall Configuration**
Now allow HTTP access:

```bash
# Allow web traffic
sudo ufw allow http
sudo ufw enable

# Verify rules
sudo ufw status
```

**Starting Your Migrated Service**
```bash
cd /opt/freshrss
sudo docker compose up -d

# Verify it's running
sudo docker ps
sudo docker logs freshrss
```

Docker compose flags:
- `-d`: Detached mode (runs in background)

**Testing Access**
1. Open your browser
2. Navigate to `http://your-server-ip`
3. Verify your feeds and settings are present
4. Test feed updates and full article fetching

**Backup Script Creation**
Create a backup script using nvim:

```bash
nvim /opt/freshrss/backup.sh
```

In nvim, enter insert mode with `i` and add:

```bash
#!/bin/bash
BACKUP_DIR="/opt/backups/freshrss"
DATE=$(date +%Y%m%d)

# Create backup directory
mkdir -p $BACKUP_DIR

# Backup FreshRSS data
tar -czf $BACKUP_DIR/freshrss-$DATE.tar.gz /opt/freshrss/freshrss/data

# Remove backups older than 7 days
find $BACKUP_DIR -type f -mtime +7 -delete
```

Save and exit with `Esc` followed by `:wq`

Make it executable and schedule:
```bash
sudo chmod +x /opt/freshrss/backup.sh

# Edit crontab with nvim
EDITOR=nvim crontab -e
```

Add this line in nvim:
```bash
0 2 * * * /opt/freshrss/backup.sh
```

Understanding permissions:
- `chmod +x`: Makes file executable
- `crontab -e`: Edit scheduled tasks

**Why This Backup Strategy?**
- Daily backups ensure minimal data loss
- Running at 2 AM minimizes impact
- Keeping 7 days of backups balances storage and safety
- Automated process ensures consistency

**Next Steps**
After successfully migrating:
1. Monitor logs for any issues
2. Test access from different devices
3. Consider setting up HTTPS for security
4. Document your cloud server details securely
