# Assignment

## 1. Background Info

- We want to extend our on-premises Active Directory DomainControler to the AWS Cloud, meaning our AD Master is on-premise
- we manage all AD DS tasks ourselves in AWS the same as in on premises
- therefore our responsibilities in this regard are:
  - EC2 Domain Controller configuration
    - DNS Configuration
    - Sites and Services Configuration
  - Monitoring
  - DC Recovery
  - Backup
  - Restore
  - Security group config
- we establish a Forest Trust Realtionship (we dont use Sync/ Replication)
  - the on-premises network is the Trusted Part
  - the AWS AD part is the Trusting Part
- Trused Users shall have cloud resource access, only if entitled by trusting admins (we control both sides)
- Resources in the cloud shall have no access to on-premises resources unless on-premises trusts the cloud AND on-premises admins grant permissions to user identities in the cloud
- we establish 2-way Trusts



## 2. Scope of delivery

| __Results__              | __Description__                                              |
| ------------------------ | ------------------------------------------------------------ |
| Documentation            | A documentation describing the resources and the approach as well as the elaborations on the deployed  AWS extensions |
| Cloudformation templates | Cloudformation templates with the resources working as expected in an automated way (vpc, ad, parameter, rdgw) |



## 3. Details of the assignment

### 3.1 General Focus

- the goal is to setup an `EC2` based Active Directory using a Microsoft 2016 Server AMI that will be deployed to enable directory-aware workloads and AWS resources  in the AWS Cloud
- it shall be used to extend our on-premises AD DS Installation to the AWS Cloud



### 3.2 Cloudformation templates

-  please provide the following templates

| template          | Content                                                      |
| ----------------- | ------------------------------------------------------------ |
| `ad_master.yaml`  | creates a nested master_stack for the following stacks       |
| `vpc.yaml`        | This template creates a Multi-AZ, multi-subnet VPC infrastructure with managed NAT gateways in the public subnet for each Availability Zone. It also includes public and private subnets with dedicated custom network access control lists (ACLs); NAT gateways are deployed to public subnets, providing outbound Internet access for the Domain Controller instances in private subnets |
| `ad.yaml`         | This template creates 2 Windows 2016 Active Directory Domain Controllers (EC2 Instances) into private subnets in separate Availability Zones inside a VPC (created with the vpc-stack). The template shall bootstrap each instance, deploying the required components, finalizing the configuration to create a new AD forest, and promoting instances in two Availability Zones to Active Directory domain controllers. The default Domain Administrator password will be the one provided via `AWS::SSM::Parameter` |
| `rdgw.yaml`       | this template deploys a Remote Desktop Gateway (RD Gateway) on the AWS Cloud in the vpc created with the vpc stack; RD Gateway uses the Remote Desktop Protocol (RDP) over HTTPS to establish a secure, encrypted connection between remote users and EC2 instances running Microsoft Windows, without needing to configure a virtual private network (VPN). |
| `parameters.yaml` | creates  the Domain Admin Password using a secret `AWS::SSM::Parameter` |

- pls use as basis for the templates the following templates provides by AWS

  https://github.com/aws-quickstart/quickstart-microsoft-activedirectory

  but remove the quick start references and embed it into its own  environment **free of the quickstart references** done by AWS

- The AWS CloudFormation templates are supposed to perform the following tasks:
  - Set up the Amazon VPC, including private and public subnets in two Availability Zones
  - Configure two NAT gateways in the public subnets
  - Configure private and public routes
  - Enable ingress traffic into the VPC for administrative access to Remote Desktop Gateway
  - Launch Windows Server 2016 AMIs
  - Configure security groups and rules for traffic between instances



### 3.3 Further details

#### 3.3.1 Use

- use a VPC and a virtual private gateway to enable communication with our own network over an IPsec VPN tunnel

- implement at least two domain controllers in the AWS Cloud environment since it provides fault tolerance and prevents a single domain controller failure from impacting the availability of the AD DS

- implement the domain controllers in at least two Availability Zones

- please  use your own AWS acccount for the development

- to generate the secret `AWS::SSM::Parameter` use the custom resource provided here:

  https://github.com/binxio/cfn-secret-provider.git

#### 3.3.2 DONT Use

- any Serverless framework boilerplates
- the **fully managed AD setup** https://docs.aws.amazon.com/quickstart/latest/active-directory-ds/scenario-3.html



## 4. Technology Stack

The following technologies are mandatory and cannot be replaced by any other technology similar or not without our explicit approval:
- AWS Cloudformation

  



## 5. Code repository

- we provide a repository named "AD"
- Fork the repository and work inside your own account
- Give access to jprivillaso@gmail.com and aerioeus@gmail.com so we can track your work
- arrange your code like this

```
--ad-masterstack
  |_ ad_master.yaml
  |_ vpc.yaml
  |_ ad.yaml
  |_ rdgw.yaml
  |_ parameters
```

- Create a Pull Request and we will review the code



## 6. Provided by us

- Credentials to connect to our on-premises Active Directory
- Follow this guide from the AWS official documentation https://docs.aws.amazon.com/quickstart/latest/active-directory-ds/scenario-2.html



### 7. Timeline

- Project Start: 16.01.2019
- Project due date:  01.02.2019
