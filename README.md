# Creating a Beginner-Friendly Dockerized Website Project

## Project Overview

We'll create a basic HTML website, containerize it with Docker, and make it accessible online using a cloud service or port forwarding.

### Step 1: Create Your Basic Website

#### First, let's create a simple website structure:
```bash
my-docker-website/
├── index.html
├── style.css
├── script.js
└── Dockerfile
```

#### Create the HTML file (index.html):
```bash
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Dockerized Website</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>Welcome to My Dockerized Website!</h1>
    </header>
    <main>
        <p>This website is running inside a Docker container!</p>
        <button onclick="showMessage()">Click me!</button>
        <div id="message"></div>
    </main>
    <script src="script.js"></script>
</body>
</html>
```

Add some styling (style.css):
```bash
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 20px;
    background-color: #f0f0f0;
}

header {
    background-color: #007acc;
    color: white;
    padding: 20px;
    text-align: center;
    border-radius: 8px;
}

main {
    margin-top: 20px;
    text-align: center;
}

button {
    background-color: #007acc;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 16px;
}

button:hover {
    background-color: #005a9e;
}

#message {
    margin-top: 20px;
    font-weight: bold;
    color: #007acc;
}
```

Add interactivity (script.js):
```bash
function showMessage() {
    document.getElementById('message').innerHTML = 
        'Hello from your containerized website! 🐳';
}
```

#### Step 2: Create the Dockerfile

#### Create a Dockerfile in your project directory:
```bash
# Use the official nginx image as base
FROM nginx:alpine

# Copy website files to nginx html directory
COPY index.html /usr/share/nginx/html/
COPY style.css /usr/share/nginx/html/
COPY script.js /usr/share/nginx/html/

# Expose port 80
EXPOSE 80

# Start nginx (this is the default command for nginx image)
CMD ["nginx", "-g", "daemon off;"]
```

Step 3: Build and Test Locally

Build the Docker image:
```bash
# Navigate to your project directory
cd my-docker-website

# Build the Docker image
docker build -t my-website .
```

Run the container locally:
```bash
# Run the container on port 8080
docker run -d -p 8080:80 --name my-website-container my-website
```

#### Using Railway (Free tier available):

    Create account at railway.app
    Install Railway CLI
    Deploy your project:

```bash
# In your project directory
railway login
railway init
railway up
```

#### Using Render (Free tier available):

    Push your code to GitHub
    Create account at render.com
    Connect your GitHub repository
    Choose "Web Service" and select your repository
    Set build command: docker build -t my-website .
    Set start command: docker run -p $PORT:80 my-website


#### Option B: Using Your Home Internet (Advanced)
#### ⚠️ Security Note: Only do this for learning purposes, not for production websites.

    Port Forwarding:
        Access your router's admin panel
        Forward external port 80 to your computer's port 8080
        Find your public IP address

    Dynamic DNS (if your IP changes):
        Use services like No-IP or DuckDNS
        Set up automatic IP updates

#### Step 5: Environment Variables and Configuration

#### Make your container more flexible by adding environment variables:

#### Updated Dockerfile with custom configuration:

```bash
FROM nginx:alpine

# Install envsubst for environment variable substitution
RUN apk add --no-cache gettext

# Copy website files
COPY index.html /usr/share/nginx/html/
COPY style.css /usr/share/nginx/html/
COPY script.js /usr/share/nginx/html/

# Copy custom nginx configuration
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

#### Create nginx.conf:
```bash
server {
    listen 80;
    server_name localhost;
    
    location / {
        root /usr/share/nginx/html;
        index index.html;
        try_files $uri $uri/ =404;
    }
    
    # Enable gzip compression
    gzip on;
    gzip_types text/plain text/css application/javascript;
}
```

#### Step 6: Docker Compose for Development

Create a docker-compose.yml for easier development:
```bash
version: '3.8'
services:
  website:
    build: .
    ports:
      - "8080:80"
    volumes:
      - ./:/usr/share/nginx/html:ro
    restart: unless-stopped
```

#### Run with: docker-compose up -d

#### Useful Docker Commands
```bash
# View running containers
docker ps

# View logs
docker logs my-website-container

# Stop container
docker stop my-website-container

# Remove container
docker rm my-website-container

# Remove image
docker rmi my-website
```

#### Important Notes

    Security: Never expose sensitive ports or services without proper security measures
    Costs: Monitor cloud service usage to avoid unexpected charges
    Backups: Always backup your code and configurations
    Monitoring: Set up basic monitoring for production deployments


#### This project gives you hands-on experience with Docker containerization, web development, and basic deployment concepts. It's an excellent foundation for learning more advanced containerization and cloud deployment techniques!
