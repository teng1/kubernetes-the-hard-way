# Kubernetes The Hard Way

Following through the GCP tutorial using ansible on local VMs running in Vagrant

## Environment prep

Prepping CoreOS to be managed by ansible takes the same bootstrapping process used in the Kubespray project 

```
# Start CoreOS hosts
Vagrant up 

# Bootstrap CoreOS hosts to correctly run ansible
ansible-playbook site.yaml -i inventory/coreos-lab/hosts.ini --become

```


for instance in node1 node2 node3; do
cat > ${instance}-csr.json <<EOF
{
  "CN": "system:node:${instance}",
  "key": {
    "algo": "rsa",
    "size": 4096
  },
  "names": [
    {
      "C": "UK",
      "L": "London",
      "O": "system:nodes",
      "OU": "Kubernetes"
    }
  ]
}
EOF



for id in 1 2 3; do
cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -hostname=node${id},172.17.8.101 \
  -profile=kubernetes \
  node${id}-csr.json | cfssljson -bare node${id}
done


cat > kube-controller-manager-csr.json <<EOF
{
  "CN": "system:kube-controller-manager",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "UK",
      "L": "London",
      "O": "system:kube-controller-manager",
      "OU": "Kubernetes"
    }
  ]
}
EOF

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -profile=kubernetes \
  kube-controller-manager-csr.json | cfssljson -bare kube-controller-manager

}

cat > kube-proxy-csr.json <<EOF
{
  "CN": "system:kube-proxy",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "UK",
      "L": "London",
      "O": "system:node-proxier",
      "OU": "Kubernetes"
    }
  ]
}
EOF

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -profile=kubernetes \
  kube-proxy-csr.json | cfssljson -bare kube-proxy

}

{

cat > kube-scheduler-csr.json <<EOF
{
  "CN": "system:kube-scheduler",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "UK",
      "L": "London",
      "O": "system:kube-scheduler",
      "OU": "Kubernetes"
    }
  ]
}
EOF

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -profile=kubernetes \
  kube-scheduler-csr.json | cfssljson -bare kube-scheduler

}


cat > kubernetes-csr.json <<EOF
{
  "CN": "kubernetes",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "UK",
      "L": "London",
      "O": "Kubernetes",
      "OU": "Kubernetes"
    }
  ]
}
EOF

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -hostname=172.17.8.101,172.17.8.102,172.17.8.103,10.0.2.15,127.0.0.1,172.17.9.1,192.168.1.212,kubernetes.default \
  -profile=kubernetes \
  kubernetes-csr.json | cfssljson -bare kubernetes

}

cat > service-account-csr.json <<EOF
{
  "CN": "service-accounts",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "UK",
      "L": "London",
      "O": "Kubernetes",
      "OU": "Kubernetes"
    }
  ]
}
EOF

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -profile=kubernetes \
  service-account-csr.json | cfssljson -bare service-account




install cfssl on master
create ca 
create other certs
distribute certs 