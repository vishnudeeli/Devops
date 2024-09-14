# Devops# Devops
man ls
ls - list of directories
pwd - present woring directory
cd - change directory
whoami - current user
sudo bash - To switch to root user
su vishnudeeli - To switch particular user
sudo useradd username - To add another user
sudo passwd username - To set password for 
sudo userdel username - To delete user

sudo touch filename.txt - to create a file
sudo vi filename - To edit file
Press escape and :wq! to save file

cat --help
cat -n filename - To show line nums

sudo cp src_filename /home/dest_filename2(dest path) - To copy files from src dir to dest dir
sudo mv src_filename /home(dest folder) - To move from sorc dir to dest dir)
sudp rm filename  - To delete files or dirs

mkdir dir_name - To create directory
mkdir -p dir_name/filename - To create directory and file
rmdir dir_name - To delete empty directories

grep search_word filename - To search word in file
grep search_word -i filename - To search case insensitive word in file

sudo sort filename - To sort lines in file

sudo chown username filename - To change the user permisions for file
chmod Number(777) filename - TO change access permissions
0- No Permissions
1- Excecute
2- Read
4- Write

id flag username - To get ids of user

tar -cvf filename foldername - To Zip a file
tar -xvf zip-tar-file - To UnZip a file

cut c1-3 filename - To show 1-3 columns in each row

sed 's/vishnu/ram' filename - Replaces the vishnu as ram in file
uniq filename - To remove duplicate rows in file

ssh master-ip - To get access to master node
ssh slave-ip - To get access to slave node
ssh-keygen - TO generate key pair

ifconfig options(-a and -s) interface

ip address - To show all connected Ips
ip link - To display link layer information

netstat -a - Display all ports liostening and non listening
netstat -at	 - Display all tcp ports

nslookup google.com - Useful get DNS info from server

curl -o URL - To transfer data from server via any protocol(Http, FTP) and saves in local machine

awk '{print}' filename - To print data line by line or to print in pattern
awk '/test/ {print}' filename - Display lines contains test

env - to display all env variables
env -i command - To run empty env
env -u variable-name - To remove variable from env

apt-get - To install and remove packages
syntax: sudo apt-get options command

ls -ltr - shows details of each file
man ls - show flags of ls
man touch - show flags of touch

Shell Scripting
-------------------------
Shell is a CLI, that takes input from user and converts into language understood by Kernel
shell Scripting - Excecuting the list of commands in defined order

df -h - disk spce
free -g - memory
nproc - no of cpus

set -x -to run code in debug mode, differentites each cmd output

ps -ef  - gives all processess running
ps -ef | grep "amazon" - gives all processing running by amazon
| - pipe used to get data from first command and send to next command
interview!Q:
date | echo "Today date" - output: Today date
Expl: date is system default cmd, using this cmd it sends output to stdin but will not able
	to receive from stdin. Pipe is only receive information when cmd is ready to pass information to next cmd.
	
ps -ef | grep "amazon" | awk -F" " '{print $2}'
It gives only second column which contains amazon

Always put at starting of file:
set -x - debug mode 
set -e - exit script when these is error
set -o pipefail -exit when pipe fails

Error logging:
curl url | grep "Error" -gives all erorrs without downloading
wget url | grep "Error" - download the file and filter errors

To find file:
sudo find / -name filename

IF-ELSE
#!/bin/bash
a=4
b=5
if [ $a -gt $b ]
then
   echo "a is greater than b"
else
   echo "b is greater than a"
fi

FOR-LOOP
#!/bin/bash

a=4
b=10

for (( i=a; i<=b; i++ ))
do
   echo $i
done


trap - to trap the signal(ctrl+c is signal)
To don't stop even press ctrl+c
trap "rm -rf*^CSIGINT 	

AccessKey  key
Secret secret key

ANSIBLE:
******************
ssh-keygen - generates private key
To access key:  cat /home/vishnudeeli/.ssh/id_rsa.pub
ssh-ed25519 <--AWS EC2 ACCESS KEY--> ubuntu@ip-172-31-34-98
steps:
1. sudo apt update
2. sudo apt install ansible
3. In Ansible server - enter ssh-keygen 
4. In Target Server - enter ssh-keygen 
5. Go to ~/.ssh/authorized_keys and Paste public key from ansible server in target server
6. In Ansible server - ssh TargetIPAdress
. Exit if you come out of server
Ansible adhoc commands:
1. create inventory file in /home/ubuntu/inventory
2. Add list of Server Ips
3. ansible -i inventory all -m shell -a "touch devopsclass" (Can give IP inplace of all)
4. Now verify devopsclass file will be created in target IP

For how 3rd command: serach for Ansible modules (to do any operations)
Ansible adhoc cmds - for only one command
Ansible play book - is for to execute multiple cmds

Grouping servers:
In inventory file:
[webservers]
Ip1
Ip2
[Appservers]
Ip3
Ip4

Now if you want to up[date only webservers:
3. ansible -i inventory webservers -m shell -a "touch devopsclass" (Can give IP inplace of all)

first-playbook.yaml
---
- name: Install and Start nginx
  hosts: all
  become: true

  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present

    - name: Start nginx
      service:
        name: nginx
        state: started

RUn cmd: ansible-playbook -i inventory first-playbook.yaml
check nginx in target server: 
	sudo systemctl status nginx
To write perfect ansible playbook use below cmd:
ansible-galaxy role init kubernetes(or any name)

Infrastucture as Code and Terraform:
*****************************************
IaC - Infrastructure as Code (IaC) automates cloud infrastructure setup using scripting rather than manual configurations.
Terraform - Terraform simplifies multi-cloud infrastructure automation by using a single tool and abstracting provider-specific details.
API as Code - Terraform utilizes API as Code to automate infrastructure setup across different cloud providers via their respective APIs.
Terraform simplifies cloud provider migration by requiring minimal changes to its scripts when moving from one provider to another.
Instead pf directly calling cloud APIs, terraform says that, write tearraform file then terraform will receive your request and convert ur requesst into API call

Terraform Steps:
WRITE: Define infrastructure in configuration files (search hashicorp terraforsm aws)
PLAN: Review the changes Terraform will make your infrastructure
APPLY: Terraform provisions your infrastructure and updates the state of file

To install Terraform: 
sudo apt-get update
sudo apt-get install -y gnupg software-properties-common curl
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt-get update
sudo apt-get install -y terraform

To create instance:
Create local state and add data into main.tf file:
https://github.com/iam-veeramalla/write_your_first_terraform_project/blob/main/aws/local_state/main.tf
terraform init
terraform plan
terraform apply


Terraform uses state file to track everything, but it stores in local machine.
We should store always in remotely(Amazon S3).
Not good idea to store in source control
Isolate and organize the state files to reduce blast radius
Do not manipulate the state file

Drawbacks:
-It is single file
-Manual changes in cloud UI cannot be identified in terraform state file.
- Not a GitOps friendly tool
- Can become very complex to manage
Explain terraform setup and what purpose of S3& Dynamo DB

CI/CD & why Jenkins & why kubernetes
***************************************
00:31 CI/CD is a crucial aspect of modern software delivery, comprising Continuous Integration and Continuous Delivery.
02:12 Continuous Integration involves integrating and testing code before delivery, while Continuous Delivery focuses on deploying code reliably and efficiently.
09:16 Key steps in CI/CD include unit testing, static code analysis, vulnerability testing, automation testing, reporting, and deployment.
12:58 Jenkins is a widely used CI/CD tool that automates the software delivery process by orchestrating various testing and deployment tasks.
17:05 CI/CD pipelines can automate the promotion of applications through different stages like Dev, Staging, and Production, ensuring consistent and reliable deployments.
23:37 Jenkins instances should ideally scale to zero when not in use to avoid unnecessary resource consumption.
26:41 Kubernetes utilizes GitHub Actions to manage CI/CD efficiently by spinning up containers only when necessary, thus optimizing resource usage.
29:30 CI/CD setups with Kubernetes and GitHub Actions allow for shared resources across projects, minimizing compute instance wastage during idle times.
30:09 Kubernetes offers scalability advantages over traditional CI/CD tools like Jenkins, enabling easy node scaling based on workload demands.
31:30 GitHub Actions provide event-driven CI/CD capabilities, contrasting with Jenkins' webhook-dependent setup, offering seamless pipeline integration across projects

Access EC2-VM from local ubuntu:
----------------------------------------------
1. create key-pair.pem in cd ~/.ssh/
2. copy your key and paste in key-pair.pem
3. Run cmd: ssh -i ~/.ssh/key-pair.pem ubuntu@34.203.208.185(public IP)
4. exit - to come out to local 

JENKINS Hands on:
folloe read.me -
https://github.com/iam-veeramalla/Jenkins-Zero-To-Hero

check port : ps -ef | grep jenkins
sudo su -
su - jenkins
docker ps - to check image

Github actions:
---------------------------
https://github.com/iam-veeramalla/GitHub-Actions-Zero-to-Hero

modify file and commit changes,
check at Repo level- green dot
DOCKER
--------------------------------------------
https://github.com/iam-veeramalla/Docker-Zero-to-Hero

DOCKER VS KUBERNETES
--------------------------------------------
Kubernetes is crucial in DevOps; job roles emphasize its importance, making it essential to learn.
02:51 🐳 Understanding Docker basics is crucial before starting with Kubernetes, as Docker is a prerequisite for Kubernetes.
05:10 🚀 Kubernetes is a container orchestration platform, unlike Docker, which is a container platform, providing automated management at scale.
08:47 ⚙️ Docker's limitations include single-host dependency, lack of auto-healing, auto-scaling, and enterprise-level support, all of which Kubernetes addresses.
19:17 🌟 Kubernetes operates in clusters, allowing for high availability and fault tolerance, unlike Docker's single-node setup in most production environments.
22:45 🔄 Kubernetes allows scaling applications using YAML files without deploying new containers, using features like ReplicaSets and Deployment YAMLs.
23:53 📈 Kubernetes supports Horizontal Pod Autoscaling (HPA) to automatically adjust the number of pods based on traffic, enhancing scalability.
25:34 🛠️ Kubernetes implements Auto Healing, where it automatically replaces failed containers or pods to maintain application availability.
28:40 🏢 Kubernetes is designed for enterprise use with robust features like load balancers, firewalls, and API gateways, unlike Docker which lacks these capabilities.

KUBERNETES ARCHITECTURE - 
------------------------
00:30 🤔 Kubernetes is referred to as "k8s" because the word "Kubernetes" has eight letters between "K" and "s".
01:42 🌐 Kubernetes offers advantages over Docker with features like cluster behavior, auto-healing, auto-scaling, and enterprise-level support.
02:21 🛠️ Kubernetes architecture components include a control plane (API server, etcd, scheduler, controller manager, cloud controller manager) and a data plane (kubelet, kube-proxy, container runtime).
04:42 🐳 In Docker, a container runs with Docker shim as the container runtime, whereas Kubernetes supports multiple container runtimes such as Docker shim, containerd, and CRI-O.
08:55 🖥️ Kubernetes control plane components include API server for exposing Kubernetes services, scheduler for pod scheduling, etcd for cluster data storage, and controller manager for managing controllers like replica sets.
20:36 🌍 Cloud controller manager in Kubernetes translates requests to cloud providers, enabling Kubernetes to manage cloud-specific resources like load balancers and storage.
22:13 🧩 Kubernetes architecture involves a master and worker nodes, with worker nodes hosting components like kubelet, Q proxy, and container runtime.
22:41 📊 API server in Kubernetes serves as the central component receiving all cluster requests, while the scheduler assigns resources across worker nodes.
22:55 🗃️ Etcd functions as Kubernetes' key-value store, storing crucial cluster information.
23:08 🕹️ Controller manager oversees Kubernetes' built-in controllers, ensuring tasks like pod scaling are managed.
23:22 🌐 Cloud controller manager facilitates interactions between Kubernetes and cloud providers, adapting requests for specific provider APIs.
DOWNLOAD KOPS
----------------
00:15 DevOps engineers managing Kubernetes in production require understanding Kubernetes distributions like EKS, OpenShift, and Rancher, not just local setups like MiniKube or k3s.
01:53 Organizations using Kubernetes expect administrators to handle cluster lifecycle management, including setup and upgrades, using production-ready distributions.
03:17 Kubernetes distributions (e.g., EKS, OpenShift) offer support and enhancements over raw Kubernetes, ensuring security patches and timely upgrades.
12:36 DevOps engineers commonly use tools like KOPS for managing Kubernetes clusters, facilitating lifecycle operations such as installation, upgrades, and configuration.
18:52 COPS (Kubernetes Operations) simplifies managing multiple Kubernetes clusters by utilizing AWS CLI and S3 buckets for configuration storage.
22:35 Use a real domain name like amazon.com or your company's domain instead of "local" domains in production Kubernetes clusters.
23:15 After creating your Kubernetes cluster, ensure to configure it properly to avoid unexpected costs or incomplete configurations.
24:12 For production, configure your purchased domain in Route 53 to complete Kubernetes cluster setup. Use "dot KH dot local" for non-production environments or testing.
https://github.com/iam-veeramalla/Kubernetes-Zero-to-Hero
cmd to setup route53 -  aws route53 create-hosted-zone --name dev.vishnu-devops-practice.com --caller-reference 1
PODS
-------------------
for cmds - https://kubernetes.io/docs/reference/kubectl/quick-reference/
00:29 🚀 Understanding Kubernetes Fundamentals
Understanding Kubernetes fundamentals is crucial before diving into application deployment.
Kubernetes provides cluster management, auto-scaling, auto-healing, and enterprise-level capabilities.
It utilizes YAML files for configuration and abstraction of container details.
02:33 🛠️ Deploying Applications in Kubernetes Pods
Kubernetes introduces the concept of Pods as the basic unit of deployment.
Pods can encapsulate one or more containers and share networking and storage resources.
YAML files specify Pod configurations, making them a declarative way to manage containers.
06:32 📑 YAML Files and Kubernetes
YAML files are essential in Kubernetes for defining configurations like Pods, Deployments, and Services.
Kubernetes promotes declarative management of resources through YAML files.
Understanding YAML syntax and structures is crucial for Kubernetes operations.
12:32 ⌨️ Interacting with Kubernetes via kubectl
kubectl is the command-line interface for interacting with Kubernetes clusters.
It allows managing nodes, pods, deployments, and other resources in Kubernetes.
Essential commands include kubectl get nodes, kubectl get pods, and kubectl apply -f.
15:25 💻 Setting Up a Local Kubernetes Cluster with Minikube
Minikube is a tool for running a single-node Kubernetes cluster locally.
Useful for learning Kubernetes concepts without cloud resources.
Installation involves straightforward steps tailored to different operating systems.
21:39 🛠️ Installation and Configuration of Kubernetes Cluster
Installation of Kubernetes using Minikube and Docker driver configuration.
Configuration verification with kubectl get nodes.
Understanding the single-node architecture of Minikube.
22:43 📦 Deploying Your First Kubernetes Pod
Creating a Kubernetes Pod using a YAML file.
Explaining the structure and key components of a Pod YAML.
Comparing Docker command equivalents (docker run) with Kubernetes Pod deployment (kubectl create -f pod.yaml).
26:12 🚀 Accessing and Verifying Kubernetes Pod
Accessing a running Kubernetes Pod using kubectl exec.
Verifying Pod status and details with kubectl get pods -o wide.
Debugging and troubleshooting Pods using kubectl logs and kubectl describe.
29:38 🔄 Scaling and Advanced Deployment in Kubernetes
Introduction to Kubernetes Deployments for scaling and advanced management.
Exploring future topics including auto-scaling and auto-healing capabilities.
Differentiating Pods from Deployments in Kubernetes architecture.


kubectl get pods -w
kubectl get nodes
kubectl get pod <name>
kubectl get deploy
kubectl get rs
kubectl get all -A
kubectl apply -f <pod-file>
kubectl delete pod <pod-name>
minikube ssh (for local minikube) 
ssh -i <file> <node-or-ipadress> (for remote)
-----------------------------------=-----------
In reallive - You will create deployment-> deployment creates replica set -> rs create pods
https://github.com/kubernetes/examples - site to get examples and to practie
----------------------------------------------
Deployments - RS - PODS
------------------------
00:56 📦 Containers run applications, managed through commands like docker run, specifying ports, volumes, and networks.
03:18 🔄 Pods in Kubernetes can host one or more containers, sharing networking and storage, suitable for interconnected application components.
05:40 🚀 Deployments in Kubernetes manage Pods, offering features like auto-scaling and auto-healing, crucial for maintaining application availability and managing high traffic.
07:56 🔄 ReplicaSets ensure Kubernetes maintains the desired number of Pods, even if some fail or are deleted, crucial for reliability and fault tolerance.
12:10 🗝️ Deployment manages ReplicaSets and Pods, automating the scaling and healing of application instances based on defined YAML specifications.
21:53 🔄 ReplicaSets ensure continuous availability by immediately replacing terminating Pods, maintaining the desired number set by the Deployment.
23:31 📈 ReplicaSets automatically scale Pods based on Deployment configurations, ensuring the specified number of Pods are always running.
24:28 🔄 Kubernetes' auto-healing feature, managed by ReplicaSets, ensures that Pods are recreated swiftly when deleted, maintaining desired cluster states.
25:10 🚀 In production, Kubernetes deployments abstract Pod management, utilizing ReplicaSets to handle Pod lifecycles and maintain application availability.
26:50 🌐 Kubernetes facilitates zero-time deployment by smoothly transitioning between Pod configurations without disrupting live traffic, ensuring application stability.
