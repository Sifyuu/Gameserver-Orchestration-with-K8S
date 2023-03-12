1. Setup firewall
    - Ufw enable
    - ufw allow ssh
    - from here do: 'ufw allow #NECCESARY_PORT#/tcp'
        - Ports that needs to be open on the master node:
            - 6443      inbound TCP for kubernetes API server
            - 2379:2380 inbound TCP for etcd server client API
            - 10250     inbound TCP for kubelet API
            - 10257     inbound TCP for kube-controller-manager
            - 10259     inbound TCP for kube-scheduler
        - Ports that needs to be open on the worker node:
            - 10250/TCP   inbound TCP for kubelet api
            - 30000:32767 inbound TCP for NodePort Servicest
    - ufw reload

2. Prefilght setup
    2.0 Disable swap
        - swapoff -a
        - nano /etc/fstab
        - comment ved swap
    2.1 setup iptables
        - text1
        - modprobe overlay
        - modprobe br_netfilter
        - text2
        - sysctl --system

3. install dependencies
    - apt-get install -y ca-certificates curl gnupg lsb-release apt-transport-https

4. setup apt repositories
    - mkdir -p /etc/apt/keyrings
    4.0 Setup containerd repo
        - curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
        - echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    4.1 Setup kubernetes repo    
        - sudo curl -fsSLo /etc/apt/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
        - echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
    - apt-get update

5. Install containerd
    - apt-get install -y containerd.io
    - containerd config default | sudo tee /etc/containerd/config.toml >/dev/null 2>&1
    - sudo sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' /etc/containerd/config.toml
    - systemctl restart containerd
    - systemctl enable containerd

6. install kubernetes components
    - apt-get install -y kubelet kubeadm kubectl
    - apt-mark hold kubelet kubeadm kubectl

- text1:
sudo tee /etc/modules-load.d/containerd.conf <<EOF
overlay
br_netfilter
EOF

- text2:
sudo tee /etc/sysctl.d/kubernetes.conf <<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF