

# network-policy


# kube-bench

- kube-bench scan
    - kubectl apply -f https://raw.githubusercontent.com/aquasecurity/kube-bench/main/job-master.yaml

# tls with nginx ingress controller

- tls03
- enable ingress in minikupe
    - minikube addons enable ingress
- kubectl -n ingress-nginx logs ingress-nginx-admission-create-nsxdh 
    - W1226 13:22:51.328981       1 client_config.go:618] Neither --kubeconfig nor --master was specified.  Using the inClusterConfig.  This might not work. {"err":"secrets \"ingress-nginx-admission\" not found","level":"info","msg":"no secret found","source":"k8s/k8s.go:229","time":"2023-12-26T13:22:51Z"} {"level":"info","msg":"creating new secret","source":"cmd/create.go:28","time":"2023-12-26T13:22:51Z"}
- kubectl -n ingress-nginx logs ingress-nginx-admission-patch-9vv5r
    - W1226 13:22:51.610608       1 client_config.go:618] Neither --kubeconfig nor --master was specified.  Using the inClusterConfig.  This might not work. {"level":"info","msg":"patching webhook configurations 'ingress-nginx-admission' mutating=false, validating=true, failurePolicy=Fail","source":"k8s/k8s.go:118","time":"2023-12-26T13:22:51Z"} {"level":"info","msg":"Patched hook(s)","source":"k8s/k8s.go:138","time":"2023-12-26T13:22:51Z"}
- create tls secret
    - openssl req -nodes -new -x509 -keyout accounting.key -out accounting.crt -subj "/CN=accounting.tls"
    - kubectl create secret tls accounting.tls --key accounting.key --cert accounting.crt
- 


# Pod should not access the metadata API

spec:
    podSelector: {}
    policyTypes:
    - Egress
    egress:
    - to:
        - ipBlock:
            cidr: 0.0.0.0/0
            except:
            - 169.254.169.254/32

# Service account for Dashboard

- Dashboard
    - Create
        - kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.6.0/aio/deploy/recommended.yaml
    - Access
        - kubectl proxy
        - http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
- Service account
- Impersonate
    - kubectl -n kubernetes-dashboard get po --as=system:serviceaccount:kubernetes-dashboard:admin-user 

- User creation
    - Client Certificate
        - User csr and key
            - openssl req -new -key scboss.key -out scboss.csr -subj “/CN=scboss/O=boss”
        - sign
            - openssl x509 -req -in scboss.csr -CA ~/.minikube/ca.crt -CAkey ~/.minikube/ca.key -CAcreateserial -out scboss.crt -days 500
    - Create user
        - kubectl config set-credentials scboss --client-certificate=scboss.crt --client-key=scboss.key
    - Switch to user

## Register a user to kubernetes

### Client cert

```bash
# User csr and key
openssl genrsa -out scboss.key 2048
openssl req -new -key scboss.key -out scboss.csr -subj "/CN=scboss/O=boss"
# Sign
openssl x509 -req -in scboss.csr -CA ~/.minikube/ca.crt -CAkey ~/.minikube/ca.key -CAcreateserial -out scboss.crt -days 500
```

### Register user to apiserver

```bash
kubectl config set-credentials scboss --client-certificate=scboss.crt --client-key=scboss.key
kubectl config set-context scboss-context --cluster=minikube --namespace=scboss --user=scboss
```

### test with raw cert and key
```bash
curl --cacert ~/.minikube/ca.crt --cert scboss.crt --key scboss.key https://192.168.49.2:8443/apis/storage.k8s.io/v1/storageclasses\?limit\=500
```


# Kube binary hash

- curl -LO "https://dl.k8s.io/v1.26.1/bin/linux/amd64/kubeadm"
- curl -LO "https://dl.k8s.io/v1.26.1/bin/linux/amd64/kubeadm.sha256"
- echo "$(cat kubeadm.sha256) kubeadm" | shasum -a 256 --check

# minikube remark

- Should run minikube start after host reboot
