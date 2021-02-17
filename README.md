By default Compose sets up a single network for your app
----


download docker and kubeadm on master and slave nodes
master and slave nodes are both ec2 instances

We have to disable swapping because, with the swap, you lose a lot of the isolation properties that make sharing machines viable. You have no predictability around performance.
swapoff -a
sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
i
 before we install dockr^^

nitialising as master.. setting pod sidr block on can comunicate to. give private ip address of master
kubeadm init --apiserver-advertise-address=<privatemasteripaddres> --pod-network-cidr=192.168.0.0/16

ignore the erroes when i included --ignorepreflighterrors

kubeadm reset to reset kubeadm init

from output, copy commands e.g
 mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

copy command/token to slave to connect to master e.g printed from master output.
kubeadm join 172.31.38.150:6443 --token 2ntm6s.i7cr9nlva1bvy1kl \
    --discovery-token-ca-cert-hash sha256:008cbb6c93a3aa98ff2a096f78e266d2c904ca0b2bd1eb0279f94951b144c1c9

now its joint the clusters

on control-plane node/master run this command to see node in cluster
kubectl get nodes

status said not ready.. so to install network plugins, use calcio plugin
commands:
from this website:
https://docs.projectcalico.org/getting-started/kubernetes/quickstart

do commands:
kubectl create -f https://docs.projectcalico.org/manifests/tigera-operator.yaml

kubectl create -f https://docs.projectcalico.org/manifests/custom-resources.yaml

Using this CNI plugin allows Kubernetes pods to have the same IP address inside the pod as they do on the VPC network. The CNI allocates AWS Elastic Networking Interfaces (ENIs) to each Kubernetes node and using the secondary IP range from each ENI for pods on the node. The CNI includes controls for pre-allocation of ENIs and IP addresses for fast pod startup times and enables large clusters of up to 2,000 nodes.

Additionally, the CNI can be run alongside Calico for network policy enforcement. The AWS VPC CNI project is open source with documentation on GitHub.
