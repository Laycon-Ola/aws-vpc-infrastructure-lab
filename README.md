# aws-vpc-infrastructure-lab
This project implements a secure and highly available AWS Virtual Private Cloud (VPC) with multiple subnets, routing configurations, security controls, and EC2 instances. It demonstrates best practices in cloud networking, infrastructure design, and environment provisioning.

🚀 Architecture Summary
      VPC Configuration
        - 3 Public Subnets
        - 4 Private Subnets
        - 2 NAT Gateways (for high availability)
        - Internet Gateway attached
      Routing
        - Public subnets route traffic via Internet Gateway
        - Private subnets route outbound traffic via NAT Gateways

************************************************************************

🔐 Security Group
      Allows inbound access on:
        - HTTPS (443)
        - SSH (22)
        - RDP (3389)
        - Custom port 9186 (nineoneeightsix)
      Network ACL
        - Applied to public subnets
        - Allows traffic on port 556

************************************************************************

🖥️ EC2 Instances
      A total of 6 EC2 instances are deployed:
        🌐 Public Subnets (3 Instances)
              - AMI: Amazon Linux
            Installed Apps:
              - Docker
            Tags:
              - Subnet: public
              - Environment: dev
        🔒 Private Subnets (3 Instances)
              - AMI: Debian
            Installed Apps:
              - Docker
              - Docker Compose
              - Helm
              - Terraform
              - Minikube
            Tags:
              - Subnet: private
              - Environment: production

************************************************************************ 

🎯 Key Features
      - High availability with multiple subnets and NAT gateways
      - Clear separation of public and private environments
      - Secure access using Security Groups and NACLs
      - Environment-based tagging strategy
      - Automated provisioning and configuration

************************************************************************

📚 What This Project Demonstrates
      - AWS networking fundamentals
      - Cloud architecture design
      - Infrastructure organization and tagging
      - DevOps tooling setup in private environments

************************************************************************

🧠 Author Notes
      This project is part of a hands-on exercise to build and understand scalable and secure AWS infrastructure.
