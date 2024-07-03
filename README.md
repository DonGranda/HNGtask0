# Project Requirements

1. AWS EC2
2. Nginx
3. Docker

# Table of Contents

1. [Set Up Your Instance](#set-up-your-instance)
2. [Running the Instance](#running-the-instance)
3. [Dockerfile Instructions](#dockerfile-instructions)
4. [How to Build and Run the Docker Container](#how-to-build-and-run-the-docker-container)
5. [Access the Webpage](#access-the-webpage)

## Set Up Your Instance

Below are the EC2 configurations:

- **Create a new EC2 instance**
  - **Instance Name**: `hangtask01`
  - **Instance Type**: `t3.medium`
  - **AMI**: `Amazon Linux 2023 AMI`
  - **Storage**: `8GB`
  - **Security Group**:
    - Create a unique key pair
    - For the security group, allow port `80` for access to Nginx and SSH access

Expand `Advanced Details` and paste the following script into the User Data section:

```sh
#!/bin/bash
sudo yum update -y 
sudo amazon-linux-extras install docker 
sudo yum install -y docker
sudo service docker start
sudo usermod -a -G docker ec2-user
```



## Dockerfile Instructions

The Dockerfile in this project is used to create a Docker image that hosts a static website using Nginx.

### Dockerfile Instructions

The Dockerfile performs the following actions:

1. **Base Image**: Starts with the official Nginx image from Docker Hub.
2. **Copy Files**: Copies the `index.html`, `styles.css`, and `jscript.js` files into the appropriate directory within the Nginx container.
3. **Expose Port**: Exposes port `80` to allow access to the website.
4. **Start Nginx**: Configures the container to run Nginx in the foreground when started.

By using this Dockerfile, you can build and run the Docker container to serve your static website quickly and reliably.

### Dockerfile Example

```Dockerfile
FROM nginx:latest

COPY index.html /usr/share/nginx/html/
COPY style.css /usr/share/nginx/html/
COPY jscript.js /usr/share/nginx/html/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

## How to Build and Run the Docker Container

1. **Save the Dockerfile** in the same directory as your `index.html`, `styles.css`, and `jscript.js` files.

2. **Open a terminal** and navigate to this directory.

3. **Build the Docker image**:

   ```sh
   docker build -t static-website:latest .
   docker run -d --name mystaticweb -p 80:80 leslie-website:latest
   ```

## Access the Webpage

Once the Docker container is running, you can access the webpage by following these steps:

1. **Open a web browser**.

2. **Navigate to the IP address of your instance**:
   
   Enter `http://<instance_ip>:80` in the address bar, replacing `<instance_ip>` with the actual IP address or hostname of your instance running the Docker container.

Ensure that port `80` is accessible based on your instance's security group settings to successfully view the website.


