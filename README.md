# Setup a Registry with SSL 

The main goal of this repository is have a guide for explain how to mount a docker Registry in K8S

## Docs


### Requirements

1. you need to have a SSL Cert and Key and a Domain Name (DNS)

### Setup

1. Create certificate definition for the registry
```
kubectl -n registry create secret tls registry-ssl --cert=ssl.cert --key=ssl.key --dry-run=client -o yaml > src/secrets-tls.yaml
```
2. Create Secrets envs with the help of the file .env
```
kubectl -n registry create secret generic registry-secrets --from-env-file=.env -o yaml --dry-run=client > src/secrets-envs.yaml
```
3. Update the file for PV and PVC objects this is for persistent data

4. create a file with apache htpasswd and storage in your pvc under /auth folder.

5. Update ingress with you domain name.

6. Create all Objects.
```
kubectl create -f src/secrets-envs.yaml
kubectl create -f src/secrets-tls.yaml
kubectl create -f src/pv/pv.yaml
kubectl create -f src/pvc/pvc.yaml
kubectl create -f src/deployment.yaml
kubectl create -f src/service.yaml
kubectl create -f src/ingress.yaml
```