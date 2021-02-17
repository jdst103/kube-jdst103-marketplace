image is product-service:latest (see jdst103-marketplace pythom app)

copy deployment yml to control-plane node.

make sure logged into docker or right permission to pull docker images


kubectl apply -f deployment.yml




sudo -i
swapoff -a
exit
strace -eopenat kubectl version

if 'The connection to the server 172.31.38.150:6443 was refused - did you specify the right host or port?'

remember if not running as root 'sudo su' it will ssay local host refused. for kubctl commands.x
---

Update the file /etc/kubernetes/manifests/kube-apiserver.yaml and add the line --service-node-port-range=0-32000.
