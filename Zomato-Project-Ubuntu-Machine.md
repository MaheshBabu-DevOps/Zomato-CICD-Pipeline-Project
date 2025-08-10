						Zomato Clone App Deployment DevOps project
						
						docker & K8S deployement on ubuntu host machine
								STAGE-1

-----------------------------------------------------------------------------------------------------
| Port               | Protocol | Purpose                                                           |
| ------------------ | -------- | ----------------------------------------------------------------- |
| **443**            | HTTPS    | Secure web traffic (SSL/TLS)                                      |
| **80**             | HTTP     | Standard web traffic                                              |
| **22**             | SSH      | Secure shell for server access                                    |
| **8080**           | TCP      | Jenkins CI/CD web interface                                       |
| **9090**           | TCP      | Prometheus monitoring interface                                   |
| **9000**           | TCP      | SonarQube code quality dashboard                                  |
| **9100**           | TCP      | Node Exporter (for Prometheus & Grafana metrics)                  |
| **3000**           | TCP      | Application front-end & Grafana dashboard                         |
| **3306**           | TCP      | MySQL database (only if app uses it — keep internal if possible)  |
| **5432**           | TCP      | PostgreSQL database (if used instead of MySQL)                    |
| **6379**           | TCP      | Redis (caching/session management if used)                        |
| **25 / 465 / 587** | TCP      | SMTP ports for sending email notifications (e.g., Jenkins alerts) |
| **8081**           | TCP      | Nexus/Artifact Repository (if storing build artifacts)            |
| **8443**           | TCP      | Alternative HTTPS for admin or APIs (if configured)               |
-----------------------------------------------------------------------------------------------------



#jenkins
Jenkins is an open-source automation server widely used to automate tasks related to building, testing, and deploying software. It is a key tool for implementing Continuous Integration (CI) and Continuous Delivery (CD) pipelines in software development. 

#install
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins


#java
sudo apt update
sudo apt install fontconfig openjdk-21-jre
java -version
openjdk version "21.0.3" 2024-04-16
OpenJDK Runtime Environment (build 21.0.3+11-Debian-2)
OpenJDK 64-Bit Server VM (build 21.0.3+11-Debian-2, mixed mode, sharing)



#start jenkins
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins


#version
jenkins --version
java --version


http://localhost:8080

sudo cat /var/lib/jenkins/secrets/initialAdminPassword



#docker

Docker helps developers build, share, run, and verify applications anywhere — without tedious environment configuration or management.Docker packages software into standardized units called containers that have everything the software needs to run including libraries, system tools, code, and runtime.


#Install
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo docker run hello-world


#start docker
sudo systemctl enable docker
sudo systemctl start docker
sudo systemctl status docker


#host
sudo usermod -aG docker jenkins


#version
docker --version
docker-compose --version



#trivy
Debian/Ubuntu
Trivy is a versatile open-source tool primarily used for vulnerability scanning and misconfiguration detection in various software development and deployment environments. It excels at identifying security weaknesses in container images, filesystems, IaC templates, and Kubernetes clusters, helping developers and security teams catch and fix issues early in the lifecycle. 


#website
https://trivy.dev/v0.18.3/installation/

#Install
sudo apt-get install wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy


#trivy --version




#Docker-Scout
Docker Scout is a software supply chain security tool developed by Docker, designed to help developers and organizations analyze, track, and secure their container images throughout the software development lifecycle. It focuses on providing visibility and control over the components, libraries, and tools used within container images.

#login
docker login
username:
passwd:


#https://docs.docker.com/scout/install/

curl -fsSL https://raw.githubusercontent.com/docker/scout-cli/main/install.sh -o install-scout.sh
sh install-scout.sh


#version
docker scout version





#sonarqube using docker
SonarQube is an open-source platform developed by SonarSource for continuous inspection of code quality. It analyzes source code to detect bugs, vulnerabilities, and code smells, helping developers improve code quality and security.


docker pull sonarqube
docker run -d --name sonarqube -p 9000:9000 sonarqube:latest
docker ps

http://localhost:9000

#Intially admin
Username: admin
Password: admin

#later
Z0mato!Clone2025





#Jenkins-setup(plugins & tools & credentials)

#How to Install Jenkins Plugins via Web UI
On the Jenkins dashboard, click Manage Jenkins on the left menu.
Click on Manage Plugins.
#In the Manage Plugins page, you will see tabs:

Updates

Available

Installed

Advanced

#Click on the Available tab to see the list of all available plugins.


#Jenkins Pipeline Plugins-------------1
Pipeline: Stage View

Pipeline: Declarative

Pipeline: Groovy

Blue Ocean

#differnet versions of java
Eclipse Temurin


#sonar
SonarQube Scanner

#Docker Plugins (Available)
Docker Pipeline
Docker Plugin
Docker API
Docker Commons
Docker
	


#Email Plugins (Available)
Email Extension template (Email-ext)

OWASP Dependency-check

NodeJS


#restart the server 


#jenkins-tools-setup------------------------------------2
http://localhost:8080/manage/configureTools/

1. JDK installations
jdk21
jdk-17.0.8.1+1


2. git
same

3. sonar
sonar-scanner

4. NodeJS installations
node24

5. Dependency-Check installations
DP-Check

6. Docker installations
docker

apply and save


#jenkins-ceredentials-----------------------3

create token in sonarqube and add into a jenkins credetials

#ID
Sonar-token


#same docker credetials  also

username
passwd


#save 


#Config sonarqube in jenkins----------------------4

http://localhost:8080/manage/configure

SonarQube servers


#sonarqube-webhook-setup
jenkins
http://localhost:8080/sonarqube-webhook







#pipeline
#Create a pipeline
Zomato-CICD-Pipeline-Project

pipeline {
    agent any
    tools {
        jdk 'jdk17'
        nodejs 'node24'
    }
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
    stages {
        stage("Clean Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Git Checkout") {
            steps {
                git branch: 'master', url: 'https://github.com/KastroVKiran/Zomato-Project-Kastro.git'
            }
        }

        stage("SonarQube Analysis") {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner \
                        -Dsonar.projectName=zomato \
                        -Dsonar.projectKey=zomato'''
                }
            }
        }

        stage("Install Dependencies") {
            steps {
                sh """
                npm install
                npm audit fix || true
                """
            }
        }

        stage("OWASP Dependency Check") {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'UNSTABLE') {
                    dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', 
                        odcInstallation: 'DP-Check'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
                }
            }
        }

        stage("Trivy Security Scan") {
            steps {
                sh 'trivy fs --security-checks vuln . > trivy.txt || true'
            }
        }

        stage("Build Docker Image") {
            steps {
                sh 'docker build -t zomato .'
            }
        }

        stage("Push to DockerHub") {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker') {
                        sh 'docker tag zomato mahi320/zomato:latest'
                        sh 'docker push mahi320/zomato:latest'
                    }
                }
            }
        }

        stage("Deploy Container") {
            steps {
                script {
                    // Remove existing container if running, then deploy on port 3001
                    sh 'docker rm -f zomato || true'
                    sh 'docker run -d --name zomato -p 3001:3000 mahi320/zomato:latest'
                }
            }
        }

        stage("Docker Scout Image") {
            steps {
                sh 'docker scout cves mahi320/zomato:latest || true'
            }
        }
    }

    post {
        always {
            echo "Pipeline completed with status: ${currentBuild.currentResult}"
        }
        success {
            echo "Application deployed successfully! Access at: http://<your-server-ip>:3001"
        }
        unstable {
            echo "Pipeline completed with warnings (OWASP scan)"
        }
        failure {
            echo "Pipeline failed. Check logs for errors."
        }
    }
}


#verfiy  zomato-app on web
http://localhost:3001


#above done by Zomato app was built, scanned, pushed to Docker Hub, and deployed into a Docker container on your Ubuntu host machine (not AWS).

All stages are ✅ green, meaning no failures.

SonarQube quality gate passed.

OWASP Dependency Check and Trivy Scan completed.

Docker image built & pushed (mahi320/zomato:latest).

The container deployed on your host machine (port 3001).



==================================================================================================================================
					#Deploying to Kubernetes on Ubuntu Host Machine (Without AWS EKS)
								STAGE-2


#note
Minikube is best for local testing (single-node).

kubeadm is better for multi-node clusters (but requires more setup).

If you later move to AWS EKS or GKE, the same Kubernetes manifests (deployment.yaml, service.yaml) will work with minimal changes.



#diff
----------------------------------------------------------------
| Feature            | Docker Container   | Kubernetes         |
| ------------------ | ------------------ | ---------------    |
| Runs containers    | ✅ Yes              | ✅ Yes           |
| Scaling            | ❌ Manual           | ✅ Automatic     |
| Self-healing       | ❌ No               | ✅ Yes           |
| Load balancing     | ❌ No               | ✅ Yes           |
| Multi-host support | ❌ Complex          | ✅ Built-in      |
| Rolling updates    | ❌ Manual, downtime | ✅ Zero downtime |
----------------------------------------------------------------



#Simple Analogy
Docker-only:
Like running a single shop yourself — you open/close it manually and manage everything yourself.

Kubernetes:
Like owning a franchise network — you have managers (K8s) to open/close shops, replace broken equipment, hire more staff if customers increase, and keep all shops running smoothly.


#Minikube
Purpose:
Lightweight, local Kubernetes cluster (mainly for development/testing).

Pros:

Easy to install (minikube start and you’re running).

Works entirely on one machine.

Has built-in addons (Ingress, metrics-server, dashboard).

Great for learning K8s basics without complex networking.

Cons:

Single-node only (even though you can simulate multiple nodes with --nodes, it’s still limited).

Not production-ready — just for local dev.

Performance limited to your local machine.



#Kubeadm
Purpose:
Tool for bootstrapping a real Kubernetes cluster — can be single-node or multi-node.

Pros:

Production-grade installation.

Same tooling and config as real multi-node clusters.

Good for learning real-world cluster setup (certificates, networking, etc.).

Cons:

More manual setup (networking plugins like Calico/Flannel, kubelet configs).

Slightly more complex to maintain.

Not as “plug-and-play” as Minikube.


==================================================================================================================================
					#Kubernetes Single Node Architecture with Zomato App Deployment



+----------------------------+
|      Ubuntu Host OS         |  <-- Physical or VM server running Ubuntu
|                            |
|  +----------------------+  | 
|  |  Container Runtime    |  |  <-- containerd runs containers
|  +----------------------+  |
|                            |
|  +----------------------+  |
|  | Kubernetes Components |  |  <-- kubelet, kubeadm, kubectl installed & running
|  +----------------------+  |
|                            |
|  +----------------------+  |
|  |  K8s Control Plane    |  |  <-- Single node acts as control plane + worker node
|  |  (kubeadm init)       |  |
|  +----------------------+  |
|                            |
|  +----------------------+  |
|  |    Pod Network (CNI)  |  |  <-- Calico networking for pod communication
|  +----------------------+  |
|                            |
|  +----------------------+  |
|  |  Zomato Kubernetes    |  |  <-- Deployment + Service running your app containers
|  |  Pods                 |  |
|  +----------------------+  |
|                            |
|  +----------------------+  |
|  | NodePort Service      |  |  <-- Exposes Zomato app on node IP:32001
|  +----------------------+  |
+----------------------------+

+-----------------------------+
| External Clients (Browser)  |  <-- Access Zomato app
|                             |
|  http://192.168.1.150:32001  |
+-----------------------------+

---

Also, **Docker standalone Zomato container** may be running on the host as well (optional):

+----------------------------+
|    Docker Standalone        |
|  Container running Zomato   |
|  on localhost:3001          |
+----------------------------+



#Common k8s Container Runtimes
containerd (recommended, default)

CRI-O (lightweight alternative)

Docker (older, deprecated as CRI but still common)

Others like kata-containers or frakti (specialized, less common)


#note
#containerd is not Kubernetes’ own software, but it is the officially recommended container runtime for Kubernetes by the Cloud Native Computing Foundation (CNCF).

----------------------------------------------------------------------------------------------
| Component       | Role                                                                     |
| --------------- | ------------------------------------------------------------------------ |
| Kubernetes      | Orchestrates containers across cluster nodes (scheduling, scaling, etc.) |
| containerd      | Runs containers on individual nodes (runtime)                            |
| CRI (Interface) | Kubernetes standard interface to talk to container runtimes              |
----------------------------------------------------------------------------------------------


#I choose containerd
What is containerd and its Purpose?
containerd is a high-level container runtime that manages the complete lifecycle of containers on a host system.


#Main Purposes of containerd:
#Pull Container Images

Downloads container images from registries (like Docker Hub).

#Manage Container Lifecycle

Creates, starts, stops, pauses, and deletes containers.

#Handle Storage & Networking

Manages container file systems and storage layers.

Integrates with network plugins to provide container networking.

#Interface for Kubernetes (CRI)

Acts as the container runtime Kubernetes talks to via the CRI (Container Runtime Interface).

It executes Kubernetes pod workloads by running container images.

#Lightweight & Efficient

Designed to be simple and stable, focusing only on container management (unlike Docker, which also includes build tools, CLI, etc.).


--------------------------------------------------------------------------------
| Purpose               | Description                                          |
| --------------------- | ---------------------------------------------------- |
| Image management      | Pulls and stores container images                    |
| Container lifecycle   | Runs, stops, and manages container processes         |
| Kubernetes runtime    | Provides Kubernetes with a container runtime via CRI |
| Lightweight execution | Runs containers efficiently without extra overhead   |
--------------------------------------------------------------------------------


#Why containerd?
It’s the core container runtime behind Docker (Docker uses containerd internally).

Kubernetes moved to use containerd directly for a lighter, more stable runtime.

It is a CNCF project, so it’s open-source and vendor-neutral.





==================================================================================================================================
								                    #process
								
Given your Zomato project is already built into a Docker image and pushed to Docker Hub, and you’re working on an Ubuntu host,


#Installation of K8S on ubuntu-machine

#1️⃣ kubeadm Setup on Ubuntu (Single Node)-Kubernetes (Single Node, kubeadm)
#Step 1: System Preparation
# Update packages
sudo apt update && sudo apt upgrade -y

# Disable swap
sudo swapoff -a
sudo sed -i '/ swap / s/^/#/' /etc/fstab

# Load required kernel modules
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF
sudo modprobe overlay
sudo modprobe br_netfilter

# Set sysctl params for Kubernetes networking
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF
sudo sysctl --system




#2️⃣ Install Container Runtime (containerd)
sudo apt install -y containerd

# Generate default containerd config
sudo mkdir -p /etc/containerd
containerd config default | sudo tee /etc/containerd/config.toml > /dev/null

# Enable systemd cgroup driver for kubelet compatibility
sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml

# Restart & enable containerd
sudo systemctl restart containerd
sudo systemctl enable containerd
sudo systemctl start containerd
sudo systemctl status containerd



#3️⃣ Install Kubernetes Components
# Install dependencies
sudo apt install -y apt-transport-https ca-certificates curl gpg

# Add Kubernetes signing key
sudo mkdir -p -m 755 /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key \
  | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

# Add Kubernetes apt repo
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] \
https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /" \
  | sudo tee /etc/apt/sources.list.d/kubernetes.list

# Install kubeadm, kubelet, kubectl
sudo apt update
sudo apt install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl


#4️⃣ Initialize Control Plane
sudo kubeadm init \
  --pod-network-cidr=192.168.0.0/16 \
  --cri-socket unix:///run/containerd/containerd.sock



#5️⃣ Configure kubectl for Your User
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config


#6️⃣ Install Calico CNI (Pod Networking)
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.2/manifests/calico.yaml



#7️⃣ (Single Node Only) Allow Scheduling on Control Plane
kubectl taint nodes --all node-role.kubernetes.io/control-plane-


#optional
kubectl run nginx --image=nginx --port=80
kubectl expose pod nginx --type=NodePort --port=80
kubectl get pods



#create a dir
cd ~
mkdir zomato-k8s
cd zomato-k8s


#How to create YAML files?
sudo nano zomato-deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: zomato-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: zomato
  template:
    metadata:
      labels:
        app: zomato
    spec:
      containers:
      - name: zomato-container
        image: mahi320/zomato:latest
        ports:
        - containerPort: 3000     # changed to 3000 to match app



sudo nano zomato-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: zomato-service
spec:
  selector:
    app: zomato
  type: NodePort
  ports:
    - protocol: TCP
      port: 3001          # external service port (can stay 3001)
      targetPort: 3000    # must match containerPort/app port inside pod
      nodePort: 32001


#Apply the YAML files
kubectl apply -f zomato-deployment.yaml
kubectl apply -f zomato-service.yaml


#note
— Kubernetes will try to pull the image specified in your deployment YAML from Docker Hub (or any other container registry if specified).


#How it works:
Your zomato-deployment.yaml references the image:

#image: mahi320/zomato:latest
When Kubernetes schedules the pod, the kubelet on the node will check if this image exists locally.

If not found locally, it will automatically pull mahi320/zomato:latest from Docker Hub.

Then it starts the container using that image.


#firewall
sudo ufw allow 32001/tcp
sudo ufw status


#Verify the pods and service
kubectl get pods-----#containers
kubectl get svc
kubectl get nodes------#physical server



#verify status
kubectl get pods -l app=zomato
kubectl logs <>
kubectl get endpoints zomato-service
kubectl get svc zomato-service



#note
Zomato app pod is now Running and Ready — that means the container pulled successfully and the app is up inside Kubernetes.


#Access your app(k8s)-deploy
ifconfig

http://192.168.1.150:32001



==================================================================================================================================================
#docker--deploy
#note
No, you do NOT need to install Docker explicitly when you install Kubernetes on Ubuntu.

Install Docker first, verify it works.

Then install Kubernetes (kubeadm, kubelet, kubectl).

If Docker stops after Kubernetes install, fix Docker by unmasking/reinstalling.
#Why does this happen?
Kubernetes cluster initialization often sets up containerd as the container runtime by default.

Sometimes Docker gets disabled or masked to avoid conflicts with containerd.

If Docker was not properly installed or configured before kubeadm, the daemon may fail to start after.

#How to fix and get Docker working after Kubernetes installation?
sudo systemctl status containerd
sudo systemctl unmask docker
sudo systemctl daemon-reload
sudo systemctl restart docker
sudo systemctl status docker

sudo apt-get install --reinstall docker-ce docker-ce-cli containerd.io
sudo systemctl restart docker



#optional 
ls -l /usr/bin/dockerd
sudo apt-get remove --purge docker docker-engine docker.io docker-ce docker-ce-cli containerd runc -y
sudo rm -rf /var/lib/docker /etc/docker
sudo apt-get update

# Install dependencies
sudo apt-get install ca-certificates curl gnupg lsb-release -y

# Add Docker’s official GPG key and repo
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

# Install Docker Engine
sudo apt-get install docker-ce docker-ce-cli containerd.io -y


sudo systemctl unmask docker
sudo systemctl enable docker
sudo systemctl start docker
sudo systemctl status docker


docker ps
docker --version


http://localhost:3001

=================================================================================================================================================

#note:-
multi-node Kubernetes cluster, you need at least two separate servers or virtual machines:

One server to run the control-plane (master) node — manages the cluster

At least one more server to run a worker node — runs your application pods



#Complete Uninstall and Cleanup of Kubernetes & Containerd on Ubuntu

#Reset kubeadm cluster state
sudo kubeadm reset -f

#Stop kubelet and containerd service
sudo systemctl stop kubelet
sudo systemctl stop containerd


#Disable kubelet and containerd services

sudo systemctl disable kubelet
sudo systemctl disable containerd


#Remove Kubernetes and containerd packages via apt (force held packages removal)

sudo apt purge -y --allow-change-held-packages kubeadm kubelet kubectl containerd
sudo apt autoremove -y


#Remove CNI configs and Kubernetes data directories

sudo rm -rf /etc/cni/net.d
sudo rm -rf /etc/kubernetes
sudo rm -rf /var/lib/etcd
sudo rm -rf /var/lib/kubelet
sudo rm -rf /var/lib/cni
sudo rm -rf /var/run/calico


#Remove containerd configuration and data

sudo rm -rf /etc/containerd
sudo rm -rf /var/lib/containerd


#Remove Kubernetes config from your home directory

rm -rf $HOME/.kube


#Remove stray kubectl, kubeadm, kubelet, containerd binaries (check and delete)
which kubectl kubeadm kubelet containerd

sudo rm -f /usr/local/bin/kubectl
sudo rm -f /home/dell/.local/bin/kubectl
sudo rm -f /usr/bin/containerd


#(Optional) Re-enable swap if you had disabled it before

sudo sed -i '/ swap / s/^#//' /etc/fstab
sudo swapon -a

sudo reboot


which kubectl kubeadm kubelet containerd
kubectl version
kubeadm version
containerd --version



my question is simle docker i installed fisert afer 
i instllef k8sok
y both are conflgicet 
telll
singlemachine
andisitneed k8s madeoty toneeddocker
=================================================================================================================================================

#note:-
#Basic commands of K8S

-------------------------------------------------------------------------------------------------------
| Command                                            | Description                                    |
| -------------------------------------------------- | ---------------------------------------------- |
| `kubectl get nodes`                                | List all nodes in the cluster                  |
| `kubectl get pods`                                 | List all pods in current namespace             |
| `kubectl get svc`                                  | List all services                              |
| `kubectl describe pod <pod>`                       | Show detailed info about a pod                 |
| `kubectl logs <pod>`                               | View logs of a pod                             |
| `kubectl create -f <file.yaml>`                    | Create resources from a YAML file              |
| `kubectl delete pod <pod>`                         | Delete a pod                                   |
| `kubectl delete svc <service>`                     | Delete a service                               |
| `kubectl apply -f <file.yaml>`                     | Apply changes from a YAML file (create/update) |
| `kubectl exec -it <pod> -- bash`                   | Access shell inside a pod container            |
| `kubectl scale deployment <name> --replicas=<num>` | Scale pods count in a deployment               |
| `kubectl get deployment`                           | List deployments                               |
-------------------------------------------------------------------------------------------------------



#example

#kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
nginx                         1/1     Running   0          149m
zomato-app-64fc466984-s98bm   1/1     Running   0          123m

#kubectl get nodes
NAME     STATUS   ROLES           AGE    VERSION
mahesh   Ready    control-plane   156m   v1.30.14




#A Pod is the smallest and simplest deployement unit in Kubernetes
If you run a Pod with an NGINX container, Kubernetes schedules that Pod on a node, assigns an IP, and manages its lifecycle.
#remove pods
kubectl delete svc nginx
kubectl delete pod nginx


#Summary: Pods are the running containers; Services are the network access points. Deleting one doesn’t automatically delete the other








#What is the Control Plane?
The Control Plane is the brain or the central management layer of a Kubernetes cluster.


#Control Plane Components (usually run on Master node)

kube-apiserver — Accepts and processes commands (API requests)

etcd — Stores cluster configuration and state data

kube-scheduler — Decides which node will run a new pod

kube-controller-manager — Ensures cluster is in desired state (e.g., maintaining replicas)

cloud-controller-manager (for cloud environments) — Manages cloud-specific logic



#This is called a single-node Kubernetes cluster where master and worker run on the same machine.


#You can verify this with:
kubectl describe node mahesh | grep Taints






#What is ArgoCD?
ArgoCD is a GitOps continuous delivery tool for Kubernetes. 
----------------------------------------------------------------------------
| Use case                      | Recommended approach                     |
| ----------------------------- | ---------------------------------------- |
| Small project, learning, demo | Manual image push + kubectl apply        |
| Growing project, team-based   | GitOps with ArgoCD + Git repo management |
| Production, multiple services | Full CI/CD pipeline + GitOps + registry  |
----------------------------------------------------------------------------




#output
----------------------------------------------------------------------------------
| Your cluster  | Role                | Description                              |
| ------------- | ------------------- | ---------------------------------------- |
| Node "mahesh" | control-plane       | Runs Kubernetes master components        |
| Node "mahesh" | also acts as worker | Runs your application pods (like zomato) |
----------------------------------------------------------------------------------



===================================================================================================================================================






































































































































































