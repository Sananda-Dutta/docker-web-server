# Docker Web Server

A comprehensive guide for containerizing a simple web application and deploying it with using Docker.

## Overview

This repository demonstrates how to containerize a simple web application using Docker and deploy it efficiently. Whether you're new to containers or looking to streamline your deployment process, this guide will walk you through the essential steps.

## Prerequisites

- Docker installed on your system ([Download Docker](https://www.docker.com/get-started))
- Basic knowledge of web development
- A text editor of your choice

## Project Structure

```
docker-web-server/
├── app/
│   ├── index.html
│   ├── style.css
│   └── script.js
├── Dockerfile
├── docker-compose.yml
├── .dockerignore
└── README.md
```

## Step 1: Create a Simple Web Application

### Create the HTML file (app/index.html)
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Docker Web Server</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>Welcome to Docker Web Server!</h1>
        <p>This web application is running inside a Docker container.</p>
        <button onclick="showMessage()">Click Me!</button>
        <div id="message"></div>
    </div>
    <script src="script.js"></script>
</body>
</html>
```

### Add styling (app/style.css)
```css
body {
    font-family: Arial, sans-serif;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
}

.container {
    background: white;
    padding: 2rem;
    border-radius: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    text-align: center;
    max-width: 500px;
}

button {
    background: #667eea;
    color: white;
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
    margin-top: 1rem;
}

button:hover {
    background: #5a6fd8;
}

#message {
    margin-top: 1rem;
    font-weight: bold;
    color: #667eea;
}
```

### Add interactivity (app/script.js)
```javascript
function showMessage() {
    const messageDiv = document.getElementById('message');
    messageDiv.innerHTML = 'Hello from your Dockerized web app! 🐳';
}
```

## Step 2: Create a Dockerfile

Create a `Dockerfile` in the root directory:

```dockerfile
# Use the official Nginx image as base
FROM nginx:alpine

# Copy the web application files to the Nginx html directory
COPY app/ /usr/share/nginx/html/

# Expose port 80
EXPOSE 80

# Start Nginx server
CMD ["nginx", "-g", "daemon off;"]
```

## Step 3: Create .dockerignore

Create a `.dockerignore` file to exclude unnecessary files:

```
node_modules
.git
.gitignore
README.md
.env
.nyc_output
coverage
.docker
```

## Step 4: Build and Run the Docker Container

### Build the Docker image
```bash
docker build -t docker-web-server .
```

### Run the container
```bash
docker run -d -p 8080:80 --name my-web-server docker-web-server
```

### Access your application
Open your browser and navigate to `http://localhost:8080`

## Step 5: Docker Compose (Optional)

Create a `docker-compose.yml` for easier management:

```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "8080:80"
    container_name: docker-web-server
    restart: unless-stopped
```

### Using Docker Compose
```bash
# Build and start
docker-compose up -d

# Stop
docker-compose down

# View logs
docker-compose logs
```

## Common Docker Commands

### Container Management
```bash
# List running containers
docker ps

# List all containers
docker ps -a

# Stop a container
docker stop my-web-server

# Remove a container
docker rm my-web-server

# View container logs
docker logs my-web-server
```

### Image Management
```bash
# List images
docker images

# Remove an image
docker rmi docker-web-server

# Pull an image
docker pull nginx:alpine
```

## Production Considerations

### Security
- Use non-root user in containers
- Scan images for vulnerabilities
- Keep base images updated
- Use multi-stage builds to reduce image size

### Performance
- Optimize Dockerfile layers
- Use appropriate base images
- Implement health checks
- Configure resource limits

### Multi-stage Dockerfile Example
```dockerfile
# Build stage
FROM node:16-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

# Production stage
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

## Deployment Options

### Cloud Platforms
- **AWS**: ECS, EKS, or Elastic Beanstalk
- **Google Cloud**: Cloud Run, GKE
- **Azure**: Container Instances, AKS
- **DigitalOcean**: App Platform, Droplets with Docker

### Container Registries
- Docker Hub
- AWS ECR
- Google Container Registry
- Azure Container Registry

## Troubleshooting

### Common Issues
1. **Port already in use**: Change the host port in the docker run command
2. **Permission denied**: Check Docker daemon is running and user permissions
3. **Container exits immediately**: Check the CMD instruction and application logs
4. **Cannot access application**: Verify port mapping and firewall settings

### Debug Commands
```bash
# Execute command in running container
docker exec -it my-web-server /bin/sh

# Inspect container details
docker inspect my-web-server

# Monitor container resource usage
docker stats my-web-server
```

## Next Steps

- Explore Kubernetes for orchestration
- Implement CI/CD pipelines
- Add monitoring and logging
- Learn about service mesh (Istio, Linkerd)
- Investigate serverless containers

## Resources

- [Docker Documentation](https://docs.docker.com/)
- [Docker Best Practices](https://docs.docker.com/develop/dev-best-practices/)
- [Nginx Docker Image](https://hub.docker.com/_/nginx)
- [Docker Compose Documentation](https://docs.docker.com/compose/)

## Contributing

Feel free to submit issues and enhancement requests!

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
