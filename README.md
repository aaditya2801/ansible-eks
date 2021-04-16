Role: ansible-eks 
=================

I have Integrated DevOps Automation Tool: Ansible with Amazon (AWS) Elastic Kubernetes Service and Deployed Multi-tier Architecture on the top of EKS Cluster. For Solving this use case I have created one Ansible Role which contains variables, files, and tasks.

## Following are the List of AWS Resources that I have deployed using this Role:
1.) Virtual Private Cloud (VPC): Isolated virtual network.
2.) Two Public Subnets in Availability Zone (AZ): ap-south-1a and ap-south-1b.
3.) Internet Gateway: That allows communication between VPC and the Internet.
4.) Route Table: This contains a set of rules, called routes, that are used to determine where network traffic from your subnet or gateway is directed.
5.) Security Group: Virtual firewall to control inbound traffic (port: 22, 80, 3306) and outbound traffic (allow all traffic).
6.) Two Roles: One for EKS that contains “AmazonEKSClusterPolicy” and another one for EC-2 that contains “AmazonEC2FullAccess”, “AmazonEKSWorkerNodePolicy” and “AmazonEC2ContainerRegistryFullAccess/ReadOnly”.
7.) EKS Cluster: It is a managed Kubernetes service that makes it easy for you to run Kubernetes on AWS and on-premises.
8.) Node Group for EKS Cluster: It automates the provisioning and lifecycle management of nodes (Amazon EC2 instances) for Amazon EKS Kubernetes clusters.

## List of Modules that I have used in Role: 
pip, Blockinfile, ec2_vpc_net, ec2_vpc_subnet, ec2_vpc_igw, ec2_vpc_route_table, ec2_group, iam_role, aws_eks_cluster, yum_repository, package, command.

## Explanation of Files that I have used in Role:
1.) policy1.json: It contains assume role policy document for service: eks.amazonaws.com.
2.) policy2.json: It contains assume role policy document for service: ec2.amazonaws.com.
3.) sc.yml: It deploy Storage Class with provisioner: Kubernetes.io/aws-ebs, type: gp2, zones: ap-south-1a/1b, iopsPerGB: 10 and fsType: ext4.
4.) secret.yml: It stores secrets in Base64 including username, password, and database name that requires in a multi-tier architecture.
5.) wordpress.yml: It creates WordPress deployment, exposes it with service type: load balancer, and claims the storage from the PersistentVolumeClaim (PVC) which is bound with AWS-EBS Storage Class.
6.) mysql.yml: It creates MySQL deployment, exposes it with service type: ClusterIP, and claims the storage from the PersistentVolumeClaim (PVC) which is bounded with AWS-EBS Storage Class.

## Explanation of Important Variables that I have used in Role:
1.) policy1: contain ARN for EKS Cluster Policy.
2.) policy2: contain ARN for EC2 Full Access.
3.) policy3: contain ARN for EKS Worker Node Policy.
4.) policy4: contain ARN for EC2 Container Registry Full Access.
5.) policy5: contain ARN for EC2 Container Registry Read Only.
6.) b_url: contain Base URL for Kubernetes Repo.
7.) g_key: contain GPG Key for Kubernetes Repo.

## Technologies Used:
* Configuration Management Tool: Ansible.
* Public Cloud: Amazon Web Services (AWS).
* Operating System: RHEL-8.

## Conclusion: 
In this role, I have covered the deployment of WordPress with MySQL Server on multi-tier architecture. It encourages the best practice of creating application components that are easy to maintain, decouple, and scale.

## Future Scope: 
I can add more AWS resources like KMS for creating and managing cryptographic keys for Security and Encrypting Volume, it can integrate with AWS CloudTrail to provide us with logs of all key usage. Replacing EBS with EFS, so PVC will be bound with AWS-EFS Storage Class are some of my Future Work for this Project.

## playbook for executing role

- hosts: localhost
  vars_prompt:
  - name: "access_key"
    prompt: "Enter access key"
    private: yes
  - name: "secret_key"
    prompt: "Enter secret key"
    private: yes
  roles:
  - role: ansible-eks
