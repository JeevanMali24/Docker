
1) What would you fo if an ec2 instance is getting slow?
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

2)If users can't access an apploication hosted on ec2, what steps would you take?
If users can't access an application on EC2, follow these simple steps:

1. **Check if the app is running**: Log in to the EC2 instance and restart the app if needed.  
2. **Check EC2 status**: Make sure the instance is running and healthy in AWS.  
3. **Check security groups**: Ensure the right ports (e.g., 80, 443) are open for access.  
4. **Check network**: Verify the IP address or DNS is correct and reachable.  
5. **Review logs**: Look at the app or system logs for errors.  
6. **Check the firewall**: Ensure no OS firewall is blocking access.  
7. **Verify domain**: If using a domain, ensure it points to the right instance or load balancer.  
8. **Ask for help**: If stuck, contact AWS support.

3)What's the difference between a Load Balancer and a Reverse Proxy?
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

4)How would you write a Terraform script to cerate an ec2 instance and run a script on every reboot?
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
