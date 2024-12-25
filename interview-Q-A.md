
## 1) What would you do if an ec2 instance is getting slow?
If an EC2 instance is getting slow, follow these simple steps:  

1. **Check Metrics**: Look at CPU, memory, disk, and network usage in CloudWatch.  
2. **Log In**: Use SSH to check running processes (`top`, `htop`) and disk usage (`df -h`).  
3. **Restart Services**: Restart any application or service causing high resource usage.  
4. **Check Logs**: Look at system and application logs for errors.  
5. **Upgrade Resources**: Resize the instance (bigger type) or add more instances with a load balancer.  
6. **Optimize Storage**: Increase disk IOPS or use SSD-backed volumes.  
7. **Optimize Apps**: Use caching or fix slow database queries.  
8. **Monitor & Secure**: Set up alarms for slow performance and check for unauthorized access.  
9. **Contact AWS**: If nothing works, ask AWS support for help.  

## 2) If users can't access an apploication hosted on ec2, what steps would you take?
If users can't access an application on EC2, follow these simple steps:

1. **Check if the app is running**: Log in to the EC2 instance and restart the app if needed.  
2. **Check EC2 status**: Make sure the instance is running and healthy in AWS.  
3. **Check security groups**: Ensure the right ports (e.g., 80, 443) are open for access.  
4. **Check network**: Verify the IP address or DNS is correct and reachable.  
5. **Review logs**: Look at the app or system logs for errors.  
6. **Check the firewall**: Ensure no OS firewall is blocking access.  
7. **Verify domain**: If using a domain, ensure it points to the right instance or load balancer.  
8. **Ask for help**: If stuck, contact AWS support.

## 3)What's the difference between a Load Balancer and a Reverse Proxy?
Here’s the simple difference between a load balancer and a reverse proxy:  

### **Load Balancer**  
- **What it does**: Spreads traffic across multiple servers to prevent overloading any one server.  
- **Why use it**: Ensures better performance and handles server failures.  

### **Reverse Proxy**  
- **What it does**: Sits in front of servers to forward client requests to the correct one.  
- **Why use it**: Adds security, hides server details, and can cache content.  

### **Key Difference**  
- A **load balancer** focuses on distributing traffic.  
- A **reverse proxy** focuses on managing and forwarding requests.  

## 4) How would you write a Terraform script to cerate an ec2 instance and run a script on every reboot?
Here’s a concise Terraform script to create an EC2 instance and run a script on every reboot:

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-12345678" # Replace with your AMI ID
  instance_type = "t2.micro"

  user_data = <<-EOT
    #!/bin/bash
    echo "This runs on every reboot" > /var/log/reboot.log
    crontab -l | { cat; echo "@reboot echo 'Rebooted' >> /var/log/reboot.log"; } | crontab -
  EOT

  tags = {
    Name = "example-instance"
  }
}
```

### Key Points:
1. **`user_data`**: Executes the script on instance initialization.
2. **`crontab`**: Sets the script to run on every reboot using `@reboot`. 

This ensures your desired commands run both during initialization and every reboot.

## 5) what is a backend in terraform, and why is it used?
In Terraform, a **backend** is a configuration that specifies where Terraform's **state file** is stored and how it is managed.  

### **Why It's Used**:  
1. **State Management**: Keeps track of resources you've created.  
2. **Remote Collaboration**: Allows multiple team members to share and manage infrastructure.  
3. **Security**: Protects sensitive data in the state file by storing it in secure remote locations.  

### **Examples of Backends**:  
- **Local**: Stores the state file on your machine.  
- **Remote**: Stores the state file in a service like AWS S3, Azure Blob, or HashiCorp Terraform Cloud.  

### **Common Remote Backend Setup (AWS S3)**:  
```hcl
terraform {
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "state/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "terraform-locks" # For locking
  }
}
```

This ensures state is safely stored and accessible for team collaboration.

## 6) what is the docker lifecycle?
The **Docker lifecycle** refers to the different stages of a container's life, from creation to termination. Here are the key stages:

1. **Image Creation**:  
   - A **Docker image** is created using a `Dockerfile` which defines the application's environment and dependencies.

2. **Image Build**:  
   - The image is built using the `docker build` command.

3. **Container Creation**:  
   - A container is created from an image using the `docker run` command.
   - The container is a running instance of the image.

4. **Container Start**:  
   - The container starts running, executing the command defined in the image.

5. **Container Running**:  
   - The container is running and can perform tasks as defined in the image.

6. **Container Stop**:  
   - The container stops running using `docker stop` or `docker kill`.

7. **Container Restart**:  
   - A stopped container can be restarted using `docker restart`.

8. **Container Removal**:  
   - After stopping, the container can be removed with `docker rm`.

9. **Image Removal**:  
   - An image can be deleted using `docker rmi` once no containers depend on it.

This is the typical lifecycle of a Docker container, from creation to removal.

## 7) what are the key docker components?
The key Docker components are:

1. **Docker Engine**:  
   - The core component responsible for running and managing containers. It has two parts:
     - **Docker Daemon (`dockerd`)**: Runs in the background and manages containers, images, and networks.
     - **Docker CLI**: Command-line interface used by users to interact with the Docker daemon.

2. **Docker Images**:  
   - Read-only templates used to create containers. They contain the application and its dependencies.

3. **Docker Containers**:  
   - Running instances of Docker images. Containers are isolated and can run applications.

4. **Docker Registries**:  
   - A repository for storing Docker images. The most common registry is **Docker Hub**, but private registries can also be used.

5. **Docker Volumes**:  
   - Persistent storage used by containers. Volumes allow data to persist beyond the lifecycle of a container.

6. **Docker Networks**:  
   - Allow containers to communicate with each other and with the outside world. Networks can be configured to provide isolation or exposure.

7. **Docker Compose**:  
   - A tool for defining and running multi-container Docker applications. It uses a `docker-compose.yml` file to configure services, networks, and volumes.

These components work together to provide a complete containerization solution.

## 8) what's the difference between a Docker image and a docker container?
The difference between a **Docker image** and a **Docker container**:

- **Docker Image**:
  - A blueprint or template that defines an application and its dependencies.
  - It is read-only and cannot be modified while running.
  - Images are used to create containers.
  
- **Docker Container**:
  - A running instance of a Docker image.
  - Containers are lightweight, isolated, and can be started, stopped, or removed.
  - They are writable and can store changes during runtime.

In simple terms:  
- **Image** is the "template," and **container** is the "running instance" of that template.

## 9) what should you do before creating a docker container?

Before creating a Docker container, you should:

1. **Install Docker**:  
   Ensure Docker is installed and running on your system.

2. **Prepare a Docker Image**:  
   - Use an existing image from Docker Hub or other registries.
   - Or, create your own image by writing a `Dockerfile` and building it with `docker build`.

3. **Check the Image**:  
   Ensure the image has the necessary dependencies and application setup.

4. **Plan Resources**:  
   Consider CPU, memory, ports, and storage requirements for the container.

5. **Set Up Networking (Optional)**:  
   Decide if the container needs specific networking (e.g., host, bridge, custom networks).

6. **Set Environment Variables (Optional)**:  
   Set any required environment variables for the application inside the container.

7. **Prepare Volumes (Optional)**:  
   If persistent storage is needed, prepare volumes or bind mounts.

Once these steps are done, you can proceed with creating and running your Docker container.

## 10) What is Docker compose and how do you use it?

### **What is Docker Compose?**  
Docker Compose is a tool used to define and run multi-container Docker applications. It uses a `docker-compose.yml` file to configure services, networks, and volumes for the containers.

---

### **How to Use Docker Compose**:

1. **Install Docker Compose**:  
   Ensure Docker Compose is installed. It is bundled with Docker Desktop or can be installed separately.

2. **Create a `docker-compose.yml` File**:  
   Define your application's services, their images, ports, volumes, and dependencies in the YAML file.  
   Example:  
   ```yaml
   version: '3'
   services:
     web:
       image: nginx
       ports:
         - "80:80"
     app:
       image: my-app-image
       depends_on:
         - db
     db:
       image: postgres
       environment:
         POSTGRES_USER: user
         POSTGRES_PASSWORD: password
   ```

3. **Run the Application**:  
   Use the command:  
   ```bash
   docker-compose up
   ```
   This starts all defined services.

4. **Manage the Application**:  
   - Stop services: `docker-compose down`.  
   - Run in the background: `docker-compose up -d`.  
   - View logs: `docker-compose logs`.

---

### **Why Use Docker Compose?**  
- Simplifies managing multi-container applications.  
- Provides easy service configuration and scaling.  
- Enables consistent setups for development and production environments.

## 11) What steps would you take if you see an "unhealthy" status in an ELB?

If an ELB (Elastic Load Balancer) shows an "unhealthy" status for targets, follow these steps:

1. **Check Target Health**:  
   - Go to the **Target Group** in the AWS Console.  
   - Verify why targets are marked unhealthy (e.g., failed health checks).

2. **Review Health Check Settings**:  
   - Ensure the **path** (e.g., `/health`) is correct and accessible.  
   - Check the **port** and **protocol** (HTTP/HTTPS).  
   - Adjust thresholds like **healthy/unhealthy thresholds** and **timeout** if needed.

3. **Verify Target Application**:  
   - Ensure the application on the targets is running and listening on the correct port.  
   - Use tools like `curl` or `telnet` to check connectivity to the health check path.

4. **Check Security Groups and Network**:  
   - Verify that the security groups allow inbound traffic on the health check port.  
   - Ensure the network ACLs and routing tables are configured correctly.

5. **Inspect Logs**:  
   - Review application logs on targets for errors.  
   - Check ELB access logs for failed requests.

6. **Check SSL/TLS Settings (if HTTPS)**:  
   - Ensure certificates are valid and correctly configured.  
   - Verify the SSL policies on the ELB.

7. **Reboot or Replace Target**:  
   - If an issue persists, restart the unhealthy instance or replace it with a new one.

8. **Test Locally**:  
   - Use a browser or API client to access the health check endpoint directly.

9. **Contact AWS Support**:  
   - If unresolved, consult AWS Support for further investigation.  

## 12) How do you optimize Docker images for better performance?

To optimize Docker images for better performance, follow these steps:

1. **Use Smaller Base Images**:  
   - Choose minimal images like `alpine` to reduce image size.

2. **Minimize Layers**:  
   - Combine commands in the `Dockerfile` to reduce the number of layers.  
   Example:  
   ```dockerfile
   RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*
   ```

3. **Avoid Unnecessary Files**:  
   - Use `.dockerignore` to exclude files not needed in the image (e.g., logs, build artifacts).

4. **Use Multi-Stage Builds**:  
   - Separate build and runtime stages to reduce the final image size.  
   Example:  
   ```dockerfile
   FROM golang AS builder
   WORKDIR /app
   COPY . .
   RUN go build -o app

   FROM alpine
   COPY --from=builder /app/app /app
   CMD ["/app"]
   ```

5. **Leverage Cached Layers**:  
   - Place commands that change less frequently earlier in the `Dockerfile`.

6. **Remove Unnecessary Packages**:  
   - Install only required dependencies and remove them after use.

7. **Use Official Images**:  
   - Prefer verified and optimized images from Docker Hub.

8. **Optimize CMD/ENTRYPOINT**:  
   - Use a single, efficient process for containers.

9. **Compress the Image**:  
   - Use tools like `docker image prune` to remove unused images.

10. **Scan for Vulnerabilities**:  
   - Use tools like `docker scan` or external tools (e.g., Trivy) to identify and fix security issues.  

These practices ensure smaller, faster, and more secure Docker images.

## 13) How would you secure a docker container?

To secure a Docker container, follow these steps:

1. **Use Official Images**:  
   - Prefer trusted and verified images from Docker Hub or private registries.

2. **Limit Privileges**:  
   - Avoid running containers as `root` by using a non-root user (`USER` directive in `Dockerfile`).

3. **Set Resource Limits**:  
   - Restrict CPU and memory usage using `--memory` and `--cpus` flags.

4. **Use Read-Only Filesystems**:  
   - Run containers with a read-only filesystem using `--read-only`.

5. **Restrict Capabilities**:  
   - Remove unnecessary Linux capabilities using `--cap-drop` and enable only required ones with `--cap-add`.

6. **Network Isolation**:  
   - Use private Docker networks to isolate containers and restrict public access.

7. **Scan Images for Vulnerabilities**:  
   - Use tools like `docker scan`, Trivy, or Clair to identify and fix vulnerabilities.

8. **Enable Logging**:  
   - Configure logging drivers to monitor container activities.

9. **Secure Secrets**:  
   - Avoid hardcoding secrets in images. Use Docker secrets or environment variables securely.

10. **Update Regularly**:  
   - Keep Docker, base images, and containers updated with the latest security patches.

11. **Use SELinux/AppArmor**:  
   - Leverage security frameworks like SELinux or AppArmor for additional isolation.

12. **Avoid Unnecessary Privileges**:  
   - Use the `--privileged` flag only when absolutely required.

These measures enhance container security and reduce vulnerabilities.
