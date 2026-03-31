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

![](media/image1.png){width="5.754861111111111in"
height="1.1923611111111112in"}

**STEP 2: I created a VPC (Isolated Network) with 6 subnets (3 public &
3 private) and a tag named - slimtech**

![](media/image2.png){width="1.7840277777777778in"
height="3.1993055555555556in"}
![](media/image3.png){width="1.6604166666666667in" height="3.20625in"}
![](media/image4.png){width="1.89375in" height="3.2194444444444446in"}

![](media/image5.png){width="5.756944444444445in" height="0.46875in"}

![](media/image6.png){width="5.758333333333334in"
height="0.32083333333333336in"}

**STEP 3: Manually created a 4^th^ Private Subnet with proper tags for
all subnets named - slimtech**

![](media/image7.png){width="5.5368055555555555in"
height="2.2465277777777777in"}

![](media/image8.png){width="5.7555555555555555in"
height="0.7340277777777777in"}

![](media/image9.png){width="5.754861111111111in" height="1.24375in"}

**STEP 4: Created the Route Table to define the network topology and
traffic path**

![](media/image10.png){width="5.754166666666666in"
height="1.9152777777777779in"}

![](media/image11.png){width="5.764583333333333in"
height="0.2923611111111111in"}

**STEP 5: Associated the public subnets with the public route table, to
define the route for my public subnets.**

![](media/image12.png){width="5.758333333333334in"
height="1.6847222222222222in"}

![](media/image13.png){width="5.768055555555556in"
height="0.7354166666666667in"}

![](media/image14.png){width="5.761111111111111in" height="0.14375in"}

**STEP 6: Created 3 NAT gateways for my private subnets per availability
zone using with proper tags, though this may be expensive but it affords
high availability and fault tolerance, preventing a single point of
failure.**

![](media/image15.png){width="4.946527777777778in"
height="2.928472222222222in"}

![](media/image16.png){width="5.754166666666666in"
height="1.2236111111111112in"}

![](media/image17.png){width="5.761111111111111in"
height="0.8770833333333333in"}

**STEP 7: Associated each NAT gateway with the route table per AZ to
enable my EC2 instances in my private subnet access to the internet.**

![](media/image18.png){width="5.761111111111111in"
height="1.3430555555555554in"}

**STEP 8: Architectural / Network Topology of my VPC. (1 route table, 1
NAT gt per AZ) to enable high availability, redundancy and fault
tolerance. I could also just have one route table to 3 NAT Gateways as
well.**

![](media/image19.png){width="5.763888888888889in"
height="1.832638888888889in"}

**STEP 9: Created my security group with access to HTTPS, RDP, SSH &
Custom Port. I allowed inbound traffic for SSH and RDP only from my
localhost public IP while letting HTTPS traffic from all IPv4 IPs and
traffic to the customized port, localized from within my vpc.**

![](media/image20.png){width="5.767361111111111in"
height="4.5159722222222225in"}

![](media/image21.png){width="5.7625in" height="1.961111111111111in"}

**STEP 10: Created a NACL for just 2 of the subnets I will be deploying
my resources to, 1 private and 1 public. This is the security or subnet
firewall that controls traffic at subnet level.**

![](media/image22.png){width="5.654861111111111in" height="1.85625in"}

**STEP 11 - Specified a rule in my NACL to allow inbound connection to
resources in public subnet listening on port 556 and 22. I believe
exposing port 556 could pose a major security risk as hackers could
potentially scan for open ports and hack into my network esp with source
IP used as 0.0.0.0/0.**

![](media/image23.png){width="5.7652777777777775in"
height="1.9840277777777777in"}

**STEP 12 - Specified a rule in my NACL to also allow outbound
connection from my subnets to the internet. It is necessary to specify
both inbound and outbound rules because NACLs are stateless by default
and don't persist data, remember user session or cache user data. A rule
therefore has to be explicitly stated both ways.**

![](media/image24.png){width="6.236111111111111in"
height="1.9159722222222222in"}

**STEP 13 - Associated NACL with a Public Subnet.**

![](media/image25.png){width="5.75625in" height="1.3006944444444444in"}

**STEP 14 - Created the public instances using a Launch Template to
configure the Amazon Linux OS, AMI, and Storage. I used a LT because I
had to create 3 instances with the same configurations, architecture and
instance types.**

![](media/image26.png){width="5.861805555555556in"
height="2.5756944444444443in"}

![](media/image27.png){width="5.009722222222222in"
height="2.9444444444444446in"}

![](media/image28.png){width="5.757638888888889in"
height="3.213888888888889in"}

![](media/image29.png){width="5.763194444444444in"
height="4.284027777777778in"}

![](media/image30.png){width="5.754166666666666in"
height="0.6736111111111112in"}

**STEP 15 - Created an ASG to configure the subnets and AZs, the EC2
public instances will be deployed to. I used an ASG because it enabled
me to select all 4 public subnets, at once that my EC2 instances will be
deployed to as well as simplifying the management of the instances.**

![](media/image31.png){width="5.261805555555555in"
height="3.452777777777778in"}

![](media/image32.png){width="5.7659722222222225in"
height="4.0569444444444445in"}

![](media/image33.png){width="5.767361111111111in"
height="4.446527777777778in"}

**STEP 16 - Added the proper tags of public and dev for the public
instances.**

![](media/image34.png){width="5.7555555555555555in"
height="1.8270833333333334in"}

**STEP 17 - Resources in my public EC2 Instances created with tags - dev
and public**

![](media/image35.png){width="5.760416666666667in"
height="0.9076388888888889in"}

**STEP 18: Changed permissions on ssh key to login to public instances**

![](media/image36.png){width="4.441666666666666in"
height="1.4319444444444445in"}

**STEP 19: Logged in securely to my first public instance showing the
docker version installed.**

![](media/image37.png){width="5.763194444444444in"
height="3.4277777777777776in"}

**Logged in securely to my second public instance showing the docker
version installed.**

![](media/image38.png){width="5.7659722222222225in"
height="3.1118055555555557in"}

**Logged in securely to my third public instance.**

![](media/image39.png){width="5.7659722222222225in"
height="3.498611111111111in"}

**STEP 20 - Created the private instances using a Launch Template to
configure the Debian OS, AMI, and Storage as well.**

![](media/image40.png){width="5.825694444444444in"
height="2.013888888888889in"}

![](media/image41.png){width="5.7625in" height="2.4833333333333334in"}

**Screenshot showing the user data for with Docker, Docker-Compose,Helm,
Minikube and Terraform installation.**

![](media/image42.png){width="4.7347222222222225in"
height="2.6680555555555556in"}

**STEP 21 - Created the ASG for the instances associated with all 4
private subnets.**

![](media/image43.png){width="5.754861111111111in"
height="3.5145833333333334in"}

![](media/image44.png){width="5.7555555555555555in"
height="3.022222222222222in"}

![](media/image45.png){width="5.75625in" height="0.2791666666666667in"}

**STEP 22 - All private instances up and running, filtered using private
and production tags.**

![](media/image46.png){width="5.752083333333333in"
height="0.9784722222222222in"}

**STEP 23 - To login to the private instances, which has no inbound
internet access. I could use my public instance as a Bastion Host to
access my private instance or Create a VPC Endpoint using an EC2
Instance Connect Endpoint Service. I opted for the former for
simplicity.**

**STEP 24 - Firstly,i copied my access key for my private instances from
my localhost to my remote public instance.**

![](media/image47.png){width="5.760416666666667in"
height="0.24305555555555555in"}

**STEP 25 - Updated the SG Inbound Rule with my current localhost public
IP as its dynamic**

![](media/image48.png){width="5.761805555555555in"
height="0.1597222222222222in"}

**STEP 26 - ssh into my public instance, using it as a bastion host/jump
box.**

![](media/image49.png){width="5.757638888888889in"
height="2.365972222222222in"}

**STEP 27 - Updated the SG and NACL ssh inbound rule with the private IP
of my public Instance (Bastion Host), this would allow communcation btw
both instances be, within my vpc network**

![](media/image50.png){width="5.752777777777778in"
height="0.42083333333333334in"}

![](media/image51.png){width="5.7659722222222225in"
height="0.11597222222222223in"}

**STEP 28 - ssh into my private instances using its access key and
private IP, from my Bastion Host private IP, keeping communication local
within my VPC network**

![](media/image52.png){width="5.754166666666666in"
height="1.7847222222222223in"}
