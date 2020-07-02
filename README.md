![](gke-ec.png)


## Setup up AWS side

```sh
kubectl get secrets/consul-ca-cert --template='{{index .data "tls.crt" }}' |
  base64 -D > consul-agent-ca.pem
```

```sh
kubectl get secrets/consul-ca-key --template='{{index .data "tls.key" }}' |
   base64 -D > consul-agent-ca-key.pem
```

```sh
consul tls cert create -server -dc=2
```

```json
{
  "bootstrap_expect": 1,
  "datacenter": "dc2-aws",
  "client_addr": "172.31.35.214",
  "ports" : {
    "http" : -1,
    "https": 8501,
    "grpc": 8502
  },
  "data_dir": "/home/ubuntu/consul-data",
  "log_level": "DEBUG",
  "node_name": "aws-ec2",
  "server": true,
  "ui": true,
  "connect": {
    "enabled": true,
    "enable_mesh_gateway_wan_federation": true
  },
  "auto_encrypt": {
    "allow_tls": true
  },
  "primary_gateways": ["34.84.126.129:443"],
  "primary_datacenter": "dc1",
  "verify_incoming": false,
  "verify_outgoing": false,
  "ca_file": "consul-agent-ca.pem",
  "cert_file": "dc2-aws-server-consul-0.pem",
  "key_file": "dc2-aws-server-consul-0-key.pem"
}
````
