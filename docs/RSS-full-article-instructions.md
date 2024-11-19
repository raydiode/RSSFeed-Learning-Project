##### Understanding RSS and Full-Text Feeds

RSS (Really Simple Syndication) feeds typically only provide article summaries. Full-text RSS means fetching the complete article content automatically. We're setting up FreshRSS with three different parsers because different websites may respond better to different parsing methods.

##### Docker Concepts Used in This Setup

Docker provides isolated environments (containers) for our applications. We use Docker Compose to manage multiple containers as a single service. Key concepts:

- `container_name`: A human-readable identifier for each container
- `ports`: Maps container ports to host ports (format: "host:container")
- `volumes`: Persists data outside containers (format: "host_path:container_path")
- `networks`: Allows containers to communicate with each other
- `restart: always`: Automatically restarts containers if they crash or after system reboot

##### The Docker Compose File Explained

```yaml

services:
  freshrss:
    image: freshrss/freshrss:latest  # Uses the latest stable FreshRSS image
    container_name: freshrss  # Names the container for easy reference
    ports:
      - "80:80"  # Maps host port 80 to container port 80 (HTTP)
    volumes:
      # Stores FreshRSS data persistently on host machine
      - ./freshrss/data:/var/www/FreshRSS/data
      # Stores extensions persistently
      - ./freshrss/extensions:/var/www/FreshRSS/extensions
    environment:
      - TZ=Europe/London  # Sets timezone
      - CRON_MIN=*/15  # Runs feed updates every 15 minutes
      - FRESHRSS_BASE_URL=http://your-server-ip  # Your server's address
    networks:
      - freshrss-network  # Joins the shared network
    restart: unless-stopped  # Restarts container unless manually stopped

  # Readability Parser Service
  read:
    image: "phpdockerio/readability-js-server"
    container_name: freshrss-read
    ports:
      - "3000:3000"  # Each parser needs a unique port
    networks:
      - freshrss-network
    restart: always

  # Mercury Parser Service
  merc:
    image: "wangqiru/mercury-parser-api"
    container_name: freshrss-merc
    ports:
      - "3001:3000"  # Note: Host port 3001 maps to container's 3000
    networks:
      - freshrss-network
    restart: always

  # Five Filters Service
  fivefilters:
    image: "heussd/fivefilters-full-text-rss:latest"
    container_name: freshrss-fivefilters
    environment:
      - FTR_ADMIN_PASSWORD=  # Empty password disables admin interface
    volumes:
      - "./rss-cache:/var/www/html/cache/rss"  # Caches parsed content
    ports:
      - "8000:80"  # Maps to standard HTTP port inside container
    networks:
      - freshrss-network
    restart: always

# Define custom network for inter-container communication
networks:
  freshrss-network:
    driver: bridge  # Standard Docker network type for container communication
```

##### Understanding Container Networking

We create a custom network (`freshrss-network`) because:
1. It allows containers to find each other by name (DNS resolution)
2. It isolates our services from other Docker containers
3. It enables secure communication between containers

##### Common Docker Commands Explained

Start the services:
```bash
docker-compose up -d
# -d flag runs containers in "detached" mode (background)
```

Stop the services:
```bash
docker-compose down
# Stops and removes containers, networks, but preserves volumes
```

View logs:
```bash
docker-compose logs -f
# -f flag "follows" the log output in real-time
```

Access container shell:
```bash
docker exec -it freshrss bash
# exec: execute command in container
# -it: interactive terminal
# bash: the command to run (shell)
```

##### Extension Settings Explained

In FreshRSS, configure these URLs:
```
Five Filters Host: http://freshrss-fivefilters:80
Readability Host: http://freshrss-read:3000
Mercury Host: http://freshrss-merc:3000
```

These URLs work because:
1. Container names become hostnames in the Docker network
2. The ports match what we defined in docker-compose
3. HTTP is used as we're in an internal network

##### How the Parsers Work Together

1. When FreshRSS fetches an RSS feed:
   - It first gets the basic feed content
   - If full-text is enabled, it tries to fetch the complete article

2. Parser selection:
   - FreshRSS tries each enabled parser in order
   - If one fails, it tries the next
   - Different parsers work better with different websites

##### Monitoring and Troubleshooting

View specific container logs:
```bash
docker-compose logs -f freshrss
# Shows only FreshRSS container logs
```

Check FreshRSS processing:
```bash
docker exec -it freshrss tail -f /var/www/FreshRSS/data/log.txt
# tail -f: continuously show new log entries
```

##### Common Issues and Solutions

1. Container won't start:
   - Check if ports are already in use: `netstat -tulpn`
   - Verify disk space: `df -h`
   - Check Docker status: `systemctl status docker`

2. Full text not loading:
   - Verify container network: `docker network inspect freshrss-network`
   - Check parser logs for errors
   - Test parser URLs directly from FreshRSS container

##### Learning Resources

Related topics to explore:
- Docker networking and container orchestration
- [[RSS Feed Plan]]
- Linux system administration
- Web scraping and content extraction
- Log management and monitoring

Remember: When making changes to docker-compose.yml, always run `docker-compose down` followed by `docker-compose up -d` to apply changes.
