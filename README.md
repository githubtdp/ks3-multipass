Multipass /K3s  Kubernetes cluster deployment 



To create a node named master, run the following multipass launch command and pass it the following flags:
* -c with the number of CPUs to allocate (1)
* -m with the amount of memory to allocate (1G). Note that you can use the following suffixes: K, M, and G
* -d with the disk space to allocate (4G). Similarly, note that you can use the following suffixes: K, M, and G
* -n with the name of the node (master)
* The name of the image (18.04)
* Nc -v host port 

Netcat (nc) command is a command-line utility for reading and writing data between two computer networks. The communication happens using either TCP or UDP.

# Launching multipass  infrastructure 
multipass launch -c 4 -m 4G -d 20G -n k3s-master  20.04
multipass launch -c 4 -m 4G -d 20G -n k3s-worker-1 20.04
multipass launch -c 4 -m 4G -d 20G -n k3s-worker-2 20.04

# verifying multipass deployment/ provisioning state 
multipass info --all

# Deploy K3s using multipass exec command to k3s-master node
 multipass exec k3s-master -- bash -c "curl -sfL https://get.k3s.io | sh -"

# Command to store the content of this file into an environment variable called TOKEN:
multipass exec k3s-master sudo cat /var/lib/rancher/k3s/server/node-token

# command to save the IP of the master node into an environment variable named IP
multipass info k3s-master | grep IPv4 | awk '{print $2}'

# command uses  multipass to integrate another worker node in the k3s master cluster  using the TOKEN and IP environment variables
 multipass exec k3s-worker-1$f -- bash -c "curl -sfL https://get.k3s.io | K3S_URL=\"https://$IP:6443\" K3S_TOKEN=\"$TOKEN\" sh -"
 done

{
multipass exec k3s-worker-1 -- \bash -c "curl -sfL https://get.k3s.io | K3S_URL=\"https://192.168.64.36:6443\" K3S_TOKEN=\"K102c0d731a99a46c9942a39ceb43898ca8e3b88afcc31a56f1e6d495d6ad5b4ff5::server:2608cc16d0b61db05d988fc6e5d7de9d\" sh -"
multipass exec k3s-worker-2 -- \bash -c "curl -sfL https://get.k3s.io | K3S_URL=\"https://192.168.64.36:6443\" K3S_TOKEN=\"K102c0d731a99a46c9942a39ceb43898ca8e3b88afcc31a56f1e6d495d6ad5b4ff5::server:2608cc16d0b61db05d988fc6e5d7de9d\" sh -"
}


# Verify your cluster- ssh into the shell 
multipass exec k3s-master -- bash

# Verify K3s Installation
systemctl status k3s

# Running k3s version
k3s -v

# list of  all current namespace 
sudo k3s kubectl get namespace

# containers are up and running in k3s instance 
sudo crictl ps

# Check cluster name in  k3s 
 sudo kubectl config current-context

 # modify the current namespace 
Sudo k3s kubectl config set-context --cluster=cluster1  --user=<user1> --namespace=<namespace>

# create/ add new namespace 
Sudo k3s kubectl create namespace  <namesoace >

# K3s  nodes running in the cluster 
sudo k3s kubectl get node 

# List all pods in the default namespace in the k3s cluster 
sudo k3s kubectl get pods

# list the requested object(s) across all namespaces in the k3s cluster 
sudo k3s kubectl get pods --all-namespaces
  
  # List all pods in ps output format with more information (such as node name)
sudo k3s  kubectl get pods -o wide
  
# List all replication controllers and services together in ps output format - cluster IP details 
  sudo k3s kubectl get rc,services
  


Clean up your cluster
1. Open a new terminal window (on the host), and then use the multipass stop command to stop your nodes: multipass stop k3s-master k3s-worker-1 k3s-worker-2
2. Delete your instances with multipass delete k3s-master k3s-worker-1 k3s-worker-2
3. Permanently delete your instances by entering: multipass purge
4. Use the multipass list command to verify that the instances were deleted:  multipass list
5.  No instances found 


