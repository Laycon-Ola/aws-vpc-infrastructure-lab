**VPC ASSIGNMENT.**

**Part A**

Create a VPC that has 3 public subnets, 4 private subnets, 2 NAT
gateway, define the routes, structure it in your own manner, create
security group that gives access to HTTPS, rdp,ssh and a custom port of
9186 named \"nineoneeightsix\" and attach it to the VPC, also create a
network acl for public subnet that allows port 556, ensure that tags are
properly written and then document everything done with
pictures/screenshots Also include an architectural diagram of the entire
system.

**Part B**

Now that the VPC is implemented, create 6 ec2 instances(3 each) and
assign them to each subnets(both private and public). For each ec2
instances, the ones in public must have docker installed, and the ones
in private subnet must have docker, docker compose, helm, terraform and
minikube installed. All Ec2 in the public subnets must be tagged with a
name and also a key value pair of Subnet:(public/private). And also
another tag of environronment dev. same thing for private subnets but an
environment tag of production. The AMI for the ec2 in the public subnet
must be Amazon linux and the AMI for private subnet must be debian.
Ensure screenshots are taken all through the process as each individual
will take present their technical documentation and how they went about
with the solution.

**STEP 1: I enabled the closest region to Nigeria to minimize latency.**

![](images/media/image1.png)

**STEP 2: I created a VPC (Isolated Network) with 6 subnets (3 public &
3 private) and a tag named - slimtech**

![](images/media/image2.png)
![](images/media/image3.png)
![](images/media/image4.png)

![](images/media/image5.png)

![](images/media/image6.png)

**STEP 3: Manually created a 4^th^ Private Subnet with proper tags for
all subnets named - slimtech**

![](images/media/image7.png)

![](images/media/image8.png)

![](images/media/image9.png)

**STEP 4: Created the Route Table to define the network topology and
traffic path**

![](images/media/image10.png)

![](images/media/image11.png)

**STEP 5: Associated the public subnets with the public route table, to
define the route for my public subnets.**

![](images/media/image12.png)

![](images/media/image13.png)

![](images/media/image14.png)

**STEP 6: Created 3 NAT gateways for my private subnets per availability
zone using with proper tags, though this may be expensive but it affords
high availability and fault tolerance, preventing a single point of
failure.**

![](images/media/image15.png)

![](images/media/image16.png)

![](images/media/image17.png)

**STEP 7: Associated each NAT gateway with the route table per AZ to
enable my EC2 instances in my private subnet access to the internet.**

![](images/media/image18.png)

**STEP 8: Architectural / Network Topology of my VPC. (1 route table, 1
NAT gt per AZ) to enable high availability, redundancy and fault
tolerance. I could also just have one route table to 3 NAT Gateways as
well.**

![](images/media/image19.png)

**STEP 9: Created my security group with access to HTTPS, RDP, SSH &
Custom Port.I allowed inbound traffic for SSH and RDP only from my
localhost public IP while letting HTTPS traffic from all IPv4 IPs and
traffic to the customized port, localized from within my vpc.**

![](images/media/image20.png)

![](images/media/image21.png)

**STEP 10: Created a NACL for just 2 of the subnets I will be deploying
my resources to, 1 private and 1 public. This is the security or subnet
firewall that controls traffic at subnet level.**

![](images/media/image22.png)

**STEP 11 - Specified a rule in my NACL to allow inbound connection to
resources in public subnet listening on port 556 and 22. I believe
exposing port 556 could pose a major security risk as hackers could
potentially scan for open ports and hack into my network esp with source
IP used as 0.0.0.0/0.**

![](images/media/image23.png)

**STEP 12 - Specified a rule in my NACL to also allow outbound
connection from my subnets to the internet. It is necessary to specify
both inbound and outbound rules because NACLs are stateless by default
and don't persist data, remember user session or cache user data. A rule
therefore has to be explicitly stated both ways.**

![](images/media/image24.png)

**STEP 13 - Associated NACL with a Public Subnet.**

![](images/media/image25.png)

**STEP 14 - Created the public instances using a Launch Template to
configure the Amazon Linux OS, AMI, and Storage. I used a LT because I
had to create 3 instances with the same configurations, architecture and
instance types.**

![](images/media/image26.png)

![](images/media/image27.png)

![](images/media/image28.png)

![](images/media/image29.png)

![](images/media/image30.png)

**STEP 15 - Created an ASG to configure the subnets and AZs, the EC2
public instances will be deployed to. I used an ASG because it enabled
me to select all 4 public subnets, at once that my EC2 instances will be
deployed to as well as simplifying the management of the instances.**

![](images/media/image31.png)

![](images/media/image32.png)

![](images/media/image33.png)

**STEP 16 - Added the proper tags of public and dev for the public
instances.**

![](images/media/image34.png)

**STEP 17 - Resources in my public EC2 Instances created with tags - dev
and public**

![](images/media/image35.png)

**STEP 18: Changed permissions on ssh key to login to public instances**

![](images/media/image36.png)

**STEP 19: Logged in securely to my first public instance showing the
docker version installed.**

![](images/media/image37.png)

**Logged in securely to my second public instance showing the docker
version installed.**

![](images/media/image38.png)

**Logged in securely to my third public instance.**

![](images/media/image39.png)

**STEP 20 - Created the private instances using a Launch Template to
configure the Debian OS, AMI, and Storage as well.**

![](images/media/image40.png)

![](images/media/image41.png)

**Screenshot showing the user data for with Docker, Docker-Compose,Helm,
Minikube and Terraform installation.**

![](images/media/image42.png)

**STEP 21 - Created the ASG for the instances associated with all 4
private subnets.**

![](images/media/image43.png)

![](images/media/image44.png)

![](images/media/image45.png)

**STEP 22 - All private instances up and running, filtered using private
and production tags.**

![](images/media/image46.png)

**STEP 23 - To login to the private instances, which has no inbound
internet access. I could use my public instance as a Bastion Host to
access my private instance or Create a VPC Endpoint using an EC2
Instance Connect Endpoint Service. I opted for the former for
simplicity.**

**STEP 24 - Firstly,i copied my access key for my private instances from
my localhost to my remote public instance.**

![](images/media/image47.png)

**STEP 25 - Updated the SG Inbound Rule with my current localhost public
IP as its dynamic**

![](images/media/image48.png)

**STEP 26 - ssh into my public instance, using it as a bastion host/jump
box.**

![](images/media/image49.png)

**STEP 27 - Updated the SG and NACL ssh inbound rule with the private IP
of my public Instance (Bastion Host), this would allow communcation btw
both instances be, within my vpc network**

![](images/media/image50.png)

![](images/media/image51.png)

**STEP 28 - ssh into my private instances using its access key and
private IP, from my Bastion Host private IP, keeping communication local
within my VPC network**

![](images/media/image52.png)
