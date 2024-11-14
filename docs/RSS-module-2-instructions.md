##### Module 2: FreshRSS Deployment

**Overview**
This module covers the deployment of FreshRSS using Docker containers. Following Module 1's environment setup, we now create and configure our RSS reader container, making it accessible through a web browser.

**Prerequisites**
- Completed Module 0 (Project Initialisation)
- Completed Module 1 (Environment Setup)
- WSL 2 running with Docker Desktop properly configured
- Project directory structure in place

**Creating the Container Configuration**
Navigate to your docker configuration directory:
```bash
cd rss-learning-project/docker-config
```

Create the necessary directories for FreshRSS:
```bash
mkdir -p freshrss/data
mkdir -p freshrss/extensions
```

The `-p` flag serves two purposes:
- Creates parent directories if they don't exist
- Prevents errors if directories already exist

These directories are crucial for:
- `data/`: Storing RSS feed configurations, user settings, and database
- `extensions/`: Housing additional FreshRSS features and capabilities

**Docker Compose Configuration**
Create and configure the docker-compose.yml file:
```bash
touch docker-compose.yml
```

Add the following configuration:
```yaml
services:
  freshrss:
    image: freshrss/freshrss:latest
    container_name: freshrss
    ports:
      - "8080:80"
    volumes:
      - ./freshrss/data:/var/www/FreshRSS/data
      - ./freshrss/extensions:/var/www/FreshRSS/extensions
    environment:
      - TZ=Europe/London
    restart: unless-stopped
```

**Understanding the Configuration**
Each element in the configuration serves a specific purpose:
- `services`: Defines the containers we want to run
- `freshrss`: Names our service
- `image`: Specifies the Docker image to use
- `container_name`: Sets a friendly name for the container
- `ports`: Maps container port 80 to host port 8080
- `volumes`: Creates persistent storage for data and extensions
- `environment`: Sets container environment variables
- `restart`: Defines container restart behaviour

**YAML Formatting**
YAML files require precise indentation:
- Each nesting level uses 2 spaces
- All items at the same level must align
- Lists (like under volumes) need consistent indentation
- Incorrect indentation will cause Docker Compose errors

**Deploying the Container**
Launch the container in detached mode:
```bash
docker compose up -d
```

Verify the container is running:
```bash
docker ps
```

**Accessing FreshRSS**
Once the container is running:
1. Open a web browser
2. Navigate to http://localhost:8080
3. You should see the FreshRSS installation page

**Container Management**
Basic Docker commands for managing your container:
```bash
# Stop the container
docker compose stop

# Start the container
docker compose start

# View container logs
docker compose logs

# Restart the container
docker compose restart
```

**Success Criteria**
- Docker Compose file correctly configured
- Container running without errors
- FreshRSS accessible via web browser
- Persistent storage directories created
- Container automatically restarts after system reboot

**Common Issues and Solutions**
- If the container won't start, check Docker Desktop is running
- If you can't access FreshRSS, verify port 8080 isn't in use
- For permission errors, check directory ownership
- For configuration errors, verify YAML indentation

**Next Steps**
After completing this module:
1. Verify container is running properly
2. Test container persistence by restarting Docker
3. Familiarise yourself with basic Docker commands
4. Prepare for Module 3: Feed Configuration

**Notes**
- Keep Docker Compose file backed up
- Monitor container logs for issues
- Consider documenting any customisations made
- Remember container security will be addressed in Module 4
