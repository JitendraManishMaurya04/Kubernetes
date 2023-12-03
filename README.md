# Kubernetes (K8S)

## Kubernetes is an open source container ORCHESTRATION tool.
  It is developed by GOOGLE
  It helps in the coordination & management of multiple containerized applications. 
  It helps managing it in different deployment environments.
  
## WHY Kubernetes:
	-> Rise in the use of Microservices and moving away from Monolithic Applications
	-> Increased usage of Containers which has led to managing 100's of containers in different environments.
	-> This led to have a container ORCHESTRATION technology.

## What Kubernetes Offers:
	-> High Availability or no Downtime
	-> High Scalibility or high Performance
    -> Disaster Recovery - backup & restore
	
## K8S Components:
	->CLUSTER: Its a grouping of nodes that run containerized apps in an efficient, automated, distributed, and scalable manner.
				A cluster can have 1 or more Nodes.
				
	->NODE: Its a server, physical or virtual machine. There can be multiple Nodes in a Cluster.
	
	->PODS: Its the smallest execution unit in K8S.
	            ##Why POD is a smallest unit not CONTAINER:
					While containers are the smallest unit to be managed in a containerized application, Kubernetes doesn't manage containers directly. Instead, Kubernetes manages pods, each of which can itself include one or more containers.
			It provides ABSTRACTION over CONTAINER. 
				This helps in solving port allocation problem when working with mutiple containers which can cause conflict in port allocation. It becomes difficult to know which PORT is free.
			POD has its own NETWORK NAMESPACE and VIRTAUL ETHERNET CONENCTION.
			POD has its sown UNIQUE IP-ADDRESS. This IP-ADDRESS is re-allocated when a POD restarts.
			
	->CONTAINER: There can be mutiple container in a POD all sharing the POD network namespace. 
				  Each POD contains a "pause" container also known as sandbox container which holds network namespace(netns).
				  pause container enables communication between containers. When a container dies and recreated it will hold the IP-address. But if the POD itself dies,the IP-Address is re-allocated.
	
	->SERVICE: Its a STATIC/PERMANENT IP-ADDRESS, that can be attched to each POD. 
			   Life-cycle of POD and SERVICE are not connected. So, even if a POD is recreated, the SERVICE and ITS IP-ADDRESS will stay.
			   It also acts as a LOAD-BALANCER. The SERVICE gets the incoming requests from the INGRESS and forwards to POD.
			
	->INGRESS: Its the in-coming traffic to the POD. The tarffic or Request first goes to INGRESS and from there its forwaded to SERVICE.
	
	->EGRESS: Its the out-coming traffic from the POD.
	
	->ConfigMap: It usually contains the configuration data of an application deployed to POD. 
				 Its connected to POD to allow using the data stored in ConfigMap.
	
	->Secret: Its similar to ConfigMap, but used to store secret data in base64 encoded format.
			   Its connected to POD to allow using the secret stored in Secret
			   
	->VOLUMES: A physical storage attached to a POD. It cna be on a local-machine(NODE) or on remote location/outside of K8S CLUSTER.
	           K8S CLUSTER doesnt manage data persistance. A user/admin is responsible for backing/replicating of data.
			   
	->DELOYMENT: Its a BLUEPRINT for deploying PODS.
	             It provides ABTRACTION over PODS.
	             To create a replica or scale up/down the required application PODS, a deployemnt is created.
				 Used for stateless applications.
				 
	->StatefulSet: Similar to DEPLOYMENT,but its used for creating stateFul Apps or DB.
	
## K8S Architecture:
	
	There are 2-Types of NODES(servers) based on MASTER-SLAVE model.
	
	->SLAVE NODES(WORKER-MACHINE):  
		It does the actual work.
		3-Processes must be intalled on every node:
			->Container-Runtime: Every PODS in a node has CONTAINER and for ther execution a Container-Runtime is required.
			
			->Kubelet: It interacts with both Container-Runtime & NODE. 
			           Kubelet is responsible for starting a POD and running Container.
					   It provides required resources like CPU/MEMORY/RAM.
						
			->Kuber-Proxy: It forwards the request from Services to POD.
						   It has a intelligent mechanism which make sure the communication happens in an efficient manner. Basically 
						   less network hop.
							
	->MASTER NODES:  
	    Does the managing processes.
		4-Process must be installed on every node.
			->API-SERVER: It act as a CLUSTER gateway. 
			              User interacts via a CLIENT(UI/Command line tool->Kubelet/Kubernetes API) to the API-SERVER.
						  Act as a GATE-KEEPER for Authentication.
			
			->Scheduler: It has a intelligent mechanism to decide to which NODE/POD the scheduling request should go.
						 It only decides to which NODE a new POD should be scheduled based on resources utilized. Actual job is done by Kubelet.
			
			
		    ->Controller-Manager:  It detects state changes in a CLUSTER. If a POD dies, it detects and reschedule it ASAP.
			                       After detecting it make request to Scheduler to reschedule the dead PODS.
			
			->etcd: Its the CLUSTER brain.
			        Cluster changes are stored in the key-value store of etcd.
					All the kubernetes mechanism happens beacuse of the data of etcd.
					Actual application data is not stored in etcd.

## Minikube:
		Minikube is a lightweight K8S implementation that creates a VM on your local machine and deploys a simple cluster containing only one node. 
		Minikube is available for Linux, macOS, and Windows systems.
		Both Master & Worker Processes run on One sungle NODE.
		Docker-Container Runtime is pre-installed
		It provides Kubectl.

## Kubectl:
	It is a command line tool for K8S helping in interacting with CLUSTER.
	All CLUSTER Activities can be manged by kubectl.
	
## COMMANDS:
		=> Starting Minicube cluster
			minicube start
		=> Stoping Minicube cluster
			minicube stop
		=> Status check of Minicube cluster
			minicube status
		
		=> Show NODES
			kubectl get nodes
		=> Show POD
			kubectl get pod
		=>COMMAND to get more INFO on a POD like its IP-ADDRESS etc
			kubectl get pod -o wide
		=> List all K8S Components
			kubectl get services
		=> List all Deployments
			kubectl get deployment
		=> COMMAND to get more INFO on a DEPLOYMENTS in YAML format
			kubectl get deployment DEPLOYMENT-NAME} -o yaml
		=> List all options for CREATE COMMAND
			kubectl create -h
		=> COMMAND to create a Deployment/POD
			kubectl create deployment NAME --image=IMAGENAME [--dry-run] [options]
			Eg: kubectl create deployment nginx-depl --image=nginx
			
			->Its a BLUEPRINT forcreating PODS
			->We can provide how many replicas of a POD we want in the deployment command
			
		=> List all Replica-Set
			kubectl get replicaset
			
			->A replicaset is present between a POD and Deployment.
			->It manages the replica of a POD
			
		=> COMMAND to EDIT a deployment
			kubectl edit deployment {DEPLOYMENT-NAME}
		
		=> COMMAND to LOG
			kubectl logs {POD-ID}
		=> COMMAND provide more DESCRIPTION of a POD/Service
			kubectl decribe pod {POD-ID}
			kubectl decribe service {SERVICE-NAME}
		
		=> COMMAND to EXECUTE a RUNNING POD in INTERACTIVE mode using POD-ID & go inside Container & open SHELL
			kubectl exec -it {POD-ID} -- bin/bash
			
		=> COMMAND to DELETE a Deployment
			kubectl delete deployment {DEPLOYMENT-NAME}
			-> It will delete the deployment and everything present inside it.
		
		=>COMMAND to CREATE/DELETE a DEPLOYMENT using a CONFIGURATION YAML file
			kubectl apply -f {filename.yaml}
			kubectl DELETE -f {filename.yaml}
		
			
## YAML CONFIG FILE
	-> Contains 3 main parts in a file
		 1) metadata
		 2) specification(spec)
		 3) status
	
	->Connection happens in the config file via label and selector field
	
## BASE-64 encoding command:
	echo -n 'username' | base64
	echo -n 'password' | base64


## Steps to connect MONGODB and MONGO-EXPRERSS
	1) Create YAML file for MONGO-DB Deployment
	2) Create YAML file for MONGO-DB Secret
	3) Add required secret Reference fields in MONGO-DB deployment file
	4) Run Below 2 commands:
		kubectl apply -f mongo-secret.yaml
		kubectl apply -f mongo-depl.yaml
	5) Internal MONGODB SERVICE Creation
		->Create either a separate service file for mongodb or add the Service config in same mongo-depl.yaml file as YAML support multiple doc
	6) kubectl apply -f mongo-depl.yaml
		->This will apply the internal service for MONGO-DB
	7) Create YAML file for MONGO-EXPRESS Deployment
	8) Create YAML file for MONGO-DB ConfigMap
	9) Add required secret & configMap Reference fields in MONGO-EXPRESS deployment file
	10) Run Below 2 commands:
		kubectl apply -f mongo-configMap.yaml
		kubectl apply -f mongo-express-depl.yaml
		
		-> after theis the Mongo-express should be able to connect to MONGO-DB
	11) External MONGO-EXPRESS SERVICE Creation
		-> Create an external service to connect from HOST System.
		-> The Service configuration is similar to Internal Service with only difference is a "type" LoadBalancer & "nodePort" field added
		->"nodePort" value should be between 30000 to 32767
		-> Not mandatory to provide nodePort. Kubernetes will provide an external port on its own
		-> "type" LoadBalancer, assigns service an external IP address and so accepts external requests
	12) kubectl apply -f mongo-express-depl.yaml
		->This will apply the external service for MONGO-DB
	13) MINIKUBE(ONLY)-> minkube service {SERVICE-NAME}
		->to assign external IP address
    14) Use the URL with externalIP and NodePort to connect via browser
		http://externalIP:nodePort

## NAMESPACE
	-> Provides Logical grouping of resources. Helps to organize resources in the namespace.
	-> Can solve Conflicts while working with multi-team which can have config with same name.
	-> Reource sharing between different namespace
	-> Access and Resource limit
	-> Cant use many resources present in 1-namespace into another like ConfigMap
	-> Cant Isolate or put some resources ina namespace like NODE, VOLUMES
	-> Can have multiple namespace in a cluster. Can think of it as a virtual cluster in K8s cluster

	=>Command to get namespaces
		
		kubectl get namespaces
		
			NAME              STATUS   AGE
			default           Active   2d22h
			kube-node-lease   Active   2d22h
			kube-public       Active   2d22h
			kube-system       Active   2d22h
	
	->kube-system: Contains info about system process like master, worker, kubectl process
	  kube-public: Contains publicly accessible data like ConfigMap which container cluster info
	  kube-node-lease: Contains info on heartbeat of nodes. Each node has its own associated lease objects which contains info on the  availabilty of that node
	  default: Resources which we create are located here if we dont create our own namespace

	
	=> COMMAND to create a namespace
		kubectl create namespace {NAMESPACE-NAME}
	=> COMMAND to apply a config file in a CUSTOM namespace
		kubectl apply -f {filename.yaml} --namespace={NAMESPACE-NAME}
	=> COMMAND to fetch anything from a CUSTOM namespace
		kubectl get all -m {NAMESPACE-NAME}
		
## EXTERNAL SERVICE VS INGRESS:
	-> External service should be used only for quick testing purpose.
	-> while accessing any application using external service http://externalIP:nodePort format is used.
	-> while using Ingress a proper domain name url is used.
	
	#Configuring Ingress
		-> A Ingress config yaml file is required to be deployed.
		-> Remove External service block and use a internal Service yaml config. Only the "type" LoadBalancer and "nodePort" fields are  required to be removed.
		-> Ingress Controller POD: 
				->A separate POD which will become the entry point of all req
				->validate/evaluate all the rules present in Ingress file.
				-> Manges redirection
		-> Need to install Ingress Conroller. K8S provides its own "NgInx Ingress Controller".
		-> Need to also have a PROXY SERVER which will open ports and have a public Ip for users. In case we are using any cloud provider, we can use its Load-Balacer instead of PROXY SERVER.
		
	#Flowdiagram using INGRESS:
		Proxy-Server/Load-Balancer => Ingress-Controller => App-Ingress-Component/Yaml Config File Rule Validation => App-Internal-Service

	
