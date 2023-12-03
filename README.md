# Kubernetes(K8S)

## Kubernetes is an open source container ORCHESTRATION tool.
  It is developed by GOOGLE
  It helps in the coordination & management of multiple containerized applications. 
  It helps managing it in different deployment environments.
  
## Why Kubernetes:
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
	
	->SERVICE: Provides a STATIC/PERMANENT IP-ADDRESS, that can be attched to each POD. 
			   Life-cycle of POD and SERVICE are not connected. So, even if a POD is recreated, the SERVICE and ITS IP-ADDRESS will stay.
			   It also acts as a LOAD-BALANCER. The SERVICE gets the incoming requests from the INGRESS and forwards to POD.
			
	->INGRESS: Its the in-coming traffic to the POD. The tarffic or Request first goes to INGRESS and from there its forwaded to SERVICE.
	
	->EGRESS: Its the out-coming traffic from the POD.
	
	->ConfigMap: It usually contains the configuration data of an application deployed to POD. 
				 Its connected to POD to allow using the data stored in ConfigMap.
	
	->Secret: Its similar to ConfigMap, but used to store secret data in base64 encoded format.
			   Its connected to POD to allow using the secret stored in Secret
			   
	->VOLUMES: A physical storage attached to a POD. It can be on a local-machine(NODE) or on remote location/outside of K8S CLUSTER.
	           K8S CLUSTER does'nt manage data persistance. A user/admin is responsible for backing/replicating of data.
			   
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
		
			
## YAML Config File
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
	-> Cant be used by multiple resources present different namespace like ConfigMap
	-> Cant isolate or put some resources in a namespace like NODE, VOLUMES
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
	-> while using Ingress a proper domain name URL is used.
	
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
		
		
## HELM & HELM-CHARTS
		#HELM:	
				1) Its a package manager for K8S.
				   Used for packaging group of YAML files and distributing them in public or private repositories.
				2) Can be used as a TEMPLATING ENGINE
					Defines a common BLUEPRINT/ TEMPLATE FILE
					Dynamic values are replaced by placeholders & this values can come from CMD line(--set flag) or values.yaml file
					## Below are two files are required for this use case, one is a Template file & other a Values file.
						-> pod-depl-template.yaml
						-> Value.yaml					
				3) Can also be used where same application is deployed across different environments
				4) Release Management
					When working with different release, HELM provides a INSATLL/UPDATE/ROLLBACK feature
						helm install <chart-name>
						helm upgrade <chart-name>
						helm rollback <chart-name>
					HELM v2 -> Contains CLIENT - SERVER(TILLER) Model
					HELM v3 -> SERVER(TILLER) is not present anymore in this version
				
				
		#HELM-CHARTS:
				-> Its a bundle of YAML files.
				-> Can Download and use already existing HELM-CHARTS created by others.
				-> Create you own HELM-CHARTS with HELM and can be pushed to a HELM Repository for making is available for others.
				-> Can get HELM-CHARTS from HELM HUB
				-> Below command can be used to look for charts
					helm search <keyword>
				-> Command to INSTALL a HELM CHART
					helm install <chart-name>
				-> Command to INSTALL a HELM CHART by overiding the values.yaml file with my-values.yaml file
					helm install --values=my-values.yaml <chart-name>
				-> Command to set a values parameter via CMD line
					helm install --set version=2.0.0
					
## K8S VOLUMES		
	-> A physical storage attached to a POD. It can be on a local-machine(NODE) or on remote location/outside of K8S CLUSTER.
	-> K8S CLUSTER does'nt manage data persistance. A user/admin is responsible for backing/replicating of data.
	-> 3 Volume Types:
		1) Persistent Volume (PV)
		2) Persistent Volume Claim (PVC)
		3) Storage Class (SC)
	
	->Storage Requirements:
		1) Storage that doesn't depend upon POD life-cycle.
		2) Storage must be available to all NODES.
		3) Storage needs to survive even if CLUSTER crashes.
		
	###Persistent Volume (PV):
	    Its not NAMESPACED.
		Accessible to whole cluster.
		
	###Persistent Volume Claim (PVC)
		Its NAMESPACED.
		Its used to claim the PV-volumes, which means an application has to claim PV-volumes using PVC-component
		Based on the Criteria i.e volume size and access Modes,defined in a PVC yaml file, a PV-Volume is assigned to the application.
		FLOW-DIAGRAM:
			POD Req Volume through PVC -> PVC tries to find a Volume in CLUSTER -> Volume has the actual storage backend
		
	###Storage Class (SC)
		Scenario where 100's of application are trying to claim a volume, it becomes difficult for admin to manually create 100's of PV.
		This issue is resolved by SC-Component and makes the process more efficient.
		SC provisions PV-Columes dynamically when PVC claims/requests it.
		FLOW-DIAGRAM:
			POD Req Volume through PVC -> PVC Request storage from SC -> SC creates PV that meets the need of Claim(PVC).
			
			
## StatefulSet
		Used for stateful applications.
		Appliations like DB, Apps that stores data
		Deployed using StatefulSet instead of Deployments
		
		### Deployment vs StatefulSet
				In StatefulSet,Replica POD's are not Identical. Each POD has its own identity i.e POD-Identity.
				StatefulSet Apps, Cant be randomly address or can't be created/deleted at same time.
				While scaling DB apps: 
					1) Node-1 contains both Read/Write Access whereas Rest Node only has Read Access to avoid data in-consistency.
					2) Node-1 acts as MASTER wheras rest acts as Worker.
					3) While scaling UP or DOWN the new POD Replicates the Data from Last POD. POD Deletion also happens one by one   from back.
					4) Whenever a new data is inserted/updated/deleted, Replica POD syncrhronizes and refreshes there data.
					5) Each POD contains its own physical storage i.e Persistent Volume(PV), but the data used is same.
					6) For data persistance for StatefulSet Apps, PV should be used and that also outside the Cluster to avoid data loss.
					7) InStatefulSet, LoadBalancer service is used same as Deployemnt, but each Node/Pod also has its own individual service name.
					8)When a StatefulSet POD restarts, due to Predictable POD-Name(eg. mysql-0, mysql-1) and fixed Individual DNS name (mysql-0.svc2, mysql-1.sv2) ,its IP-Address changes but name and endpoint stays same
					

## K8S Services
		-> In K8S, each POD has its own IP-Address, but POD's are ephemeral i.e POD are destroyed frequently. So, when a POD restarts a new IP-address is allocated. So it becomes difficult to work with POD-IP's.
		-> Services provides below features along with overcoming above issue:
			1) Static/Permanent Ip
			2) Load Balancing
			3) Loosly Coupled
			4) Can be used Within or Outside cluster
		-> Types of Services:
			1) Cluster IP Services
			2) Multi-Port Services
			3) Headless Services
				When Client wants to communicate with a POD directly.
				Use Case- StatefulSet application like DB which has POD-Identity. 
						  By Setting 'ClusterIP: None' in  Service under Spec field. Then doing a DNS lookupto get list of POD IP-Addresses and use that to communicate to specific one
			4) NodePort Services
				Used for External services where we can define a nodePort field ranging from 30000 to 32767
				Its an Extension of ClusterIP Service
			5) LoadBalancer Services
				Can use an external loadbalancer from Cloud Provider to get the request at a defined nodePort of Loadbalancer-Service
				Its an Extension of NodePort Service
		
