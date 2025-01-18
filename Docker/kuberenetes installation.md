This step-by-step guide outlines how to install Kubernetes on all nodes, configure them properly, and set up networking for Kubernetes pods. It includes the installation of required components, disabling swap, and ensuring the necessary kernel modules are loaded for Kubernetes to work. Additionally, it guides you through the process of joining worker nodes to the cluster, initializing the control-plane node, and setting up Flannel as the pod network.

Here’s a breakdown of the commands:

### Preparation
1. **Update system packages:**
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. **Install required dependencies:**
   ```bash
   sudo apt install apt-transport-https curl -y
   ```

### Install and Configure containerd (Container Runtime)
3. **Install containerd:**
   ```bash
   sudo apt install containerd -y
   ```
4. **Create configuration directory:**
   ```bash
   sudo mkdir -p /etc/containerd
   ```
5. **Generate default containerd config:**
   ```bash
   containerd config default | sudo tee /etc/containerd/config.toml > /dev/null
   ```
6. **Modify config file to enable systemd for cgroup management:**
   ```bash
   sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml
   ```
7. **Restart containerd to apply changes:**
   ```bash
   sudo systemctl restart containerd
   ```

### Install Kubernetes Components
8. **Add Kubernetes apt repository key:**
   ```bash
   curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
   ```
9. **Add Kubernetes apt repository:**
   ```bash
   echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
   ```
10. **Update the package list:**
    ```bash
    sudo apt update
    ```
11. **Install Kubernetes components (kubelet, kubeadm, kubectl):**
    ```bash
    sudo apt install -y kubelet kubeadm kubectl
    ```
12. **Prevent upgrading Kubernetes components by marking them as held:**
    ```bash
    sudo apt-mark hold kubelet kubeadm kubectl
    ```

### Disable Swap (Important for Kubernetes)
13. **Turn off swap:**
    ```bash
    sudo swapoff -a
    ```
14. **Prevent swap from being enabled after a reboot:**
    ```bash
    sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
    ```

### Load Kernel Modules
15. **Load overlay module:**
    ```bash
    sudo modprobe overlay
    ```
16. **Load br_netfilter module:**
    ```bash
    sudo modprobe br_netfilter
    ```

### Set Required Sysctl Parameters
17. **Set sysctl parameters for Kubernetes networking:**
    ```bash
    cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
    net.bridge.bridge-nf-call-iptables  = 1
    net.bridge.bridge-nf-call-ip6tables = 1
    net.ipv4.ip_forward                 = 1
    EOF
    ```
18. **Apply the sysctl changes:**
    ```bash
    sudo sysctl --system
    ```

### Initialize the Kubernetes Master (Control-Plane Node)
19. **Initialize the Kubernetes control-plane node with the Flannel network CIDR:**
    ```bash
    sudo kubeadm init --pod-network-cidr=10.244.0.0/16
    ```

### Configure kubectl for the Current User
20. **Create the kube directory for the current user:**
    ```bash
    mkdir -p $HOME/.kube
    ```
21. **Copy the admin kubeconfig file:**
    ```bash
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    ```
22. **Change ownership of the kubeconfig file to the current user:**
    ```bash
    sudo chown $(id -u):$(id -g) $HOME/.kube/config
    ```

### Join Worker Nodes to the Cluster
Run the `kubeadm join` command on each worker node that you want to add to the cluster. This command is generated after running the `kubeadm init` command on the master node (control-plane node).

Example join command:
```bash
kubeadm join 172.31.43.52:6443 --token axrhhw.h40dd662p18534ag \
        --discovery-token-ca-cert-hash sha256:3e6c3faf4a28a06743c227f8b6b6198e3b8c2ba684fd70bf0c9f168deda8fb15
```

### Install Flannel Pod Network (for Networking)
23. **Install Flannel network plugin for Kubernetes:**
    ```bash
    kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml
    ```

### Verify the Cluster
24. **Check the status of nodes in the cluster:**
    ```bash
    kubectl get nodes
    ```

25. **Monitor node status and cluster health:**
    ```bash
    watch kubectl get nodes
    ```

This process sets up Kubernetes with containerd as the container runtime, initializes the master node, and joins worker nodes to the cluster. Flannel is used for pod networking.
