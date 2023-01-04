# BastionHost-NATGateway-PortForwading-Project

# In this project we are going to use Bastion Host to SSH into a private instance, we also going to attach NAT Gateway to the private instance to enable internet access.

# STEP1: Create the Bastion Host:
Launch an instance called Bastion Host OS ubuntu 18.04  t2 micro, keypair: myprojectkeypair.pem, in public subnet.
Enable public ip and Security group: BastionHostSG SSH opened anywhere ipv4. 
Then launch.
# STEP2: Create private instance:
Launch an instance called webapp OS AWS Linux 2 t2 micro in private subnet, same keypair as Bastion Host, 
Disable public IP 
Add Security Group : SSH source: custom BastionHostSG ( to access only through Bastion Host)
Then launch.

# STEP3: PORT-FOWARDING:
From your gitbash or VS-CODE terminal cd into Downloads the create a SSH agent:    exec ssh-agent bash
Then check if the agent was successfuly created:    eval 'ssh-agent -s' it will show the SSH_AGENT_PID# then run: ssh-agent bash
To add the keypair to the SSH agent by running this command: ssh-add -k ./myprojectkeypair.pem
to connect to your Bastion Host instance using the Agent, run this command: ssh -A -i myprojectkeypair.pem ubuntu@"BastionHostpublicip"
We can check internet connectivity running: ping www.google.com
then we can connect to the webapp instance: ssh ec2-user@"weappPrivateIp"
If we check the internet access: ping www.google.com nothing happens, to enable internet access to the webapp instance we need to create NAT Gateway in public subnet,
and route the traffic into private subnet where the webapp instance is located.

# STEP4: Create NAT Gateway:
under VPC create NAT gateway in public Subnet generate Elastic IP.
Then under Route Table select the private route table ( associated with private subnet where webbapp is located) clic to route and edit route, add new rule destination: 0.0.0.0/0 source: NAT-gateway and save.
Then go back to your webapp instance terminal and check the internet connection again: ping www.google.com internet is enable!!!!

Terminate your instances and shut-down your NAT and Elastic IP

### END OF THE PROJECT
