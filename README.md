# kubernetes-jumpserver
The kubernetes cluster deploys jumpserver

USE:
- [JumpServer Docs](https://jumpserver.readthedocs.io/zh/master/)
- [aws-efs-csi-driver](https://github.com/kubernetes-sigs/aws-efs-csi-driver)

# Installation
```
git clone https://github.com/BigbigY/kubernetes-jumpserver.git
```

## Configuration

kubernetes-jumpserver/jms-secret.yaml
Secret -key, bootstrap-Token, db-password value modification
```
# Generation command： cat /dev/urandom | tr -dc A-Za-z0-9 head -c 50
secret-key: Hq6UiPpwr2OZmkBRLl1yum4SiaX2rzYtPplFK0laQNbi7V6ToO
# Generation command： cat /dev/urandom | tr -dc A-Za-z0-9 | head -c 16
bootstrap-token: QFjM8gc0utPdU7Zv
db-password: Sis1X2rzY
```
kubernetes-jumpserver/jms-core-deployment.yaml
Modify redis connections
```
- name: CORE_HOST
  value: http://core.ops:8080
- name: REDIS_HOST
  value: jumpserver.cache.amazonaws.com
- name: REDIS_PORT
  value: "6379"
```

kubernetes-jumpserver/jms-koko-deployment.yaml
Modify redis and DB connections
```
- name: DB_HOST
  value: jumpserver.rds.amazonaws.com
- name: DB_PORT
  value: "3306"
- name: DB_USER
  value: jumpserver_usro
- name: DB_NAME
  value: jumpserver
- name: REDIS_HOST
  value: jumpserver.cache.amazonaws.com
- name: REDIS_PORT
  value: "6379" 
```

## Deploy in the Kubernetes cluster
```
cd kubernetes-jumpserver
kubectl apply -f .
```


# To solve the problem of distributed file storage

EFS Deploy the driver:
```
kubectl apply -k "github.com/kubernetes-sigs/aws-efs-csi-driver/deploy/kubernetes/overlays/stable/?ref=master"
```

Deploy in the Kubernetes cluster
```
kubectl apply -f efs/

```
