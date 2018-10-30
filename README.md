## Retrieve keys from kops
```
aws s3 sync s3://kops-state-b429b/kubernetes.newtech.academy/pki/private/ca/ ca-key
aws s3 sync s3://kops-state-b429b/kubernetes.newtech.academy/pki/issued/ca/ ca-crt
mv ca-key/*.key ca.key
mv ca-crt/*.crt ca.crt
```
## Create new user
```
sudo apt install openssl
openssl genrsa -out james.pem 2048
openssl req -new -key james.pem -out james-csr.pem -subj "/CN=james/O=myteam/"
openssl x509 -req -in james-csr.pem -CA ca.crt -CAkey ca.key -CAcreateserial -out james.crt -days 10000
```

## add new context
```
kubectl config set-credentials james --client-certificate=james.crt --client-key=james.pem
kubectl config set-context james --cluster=test.aheadlabs.io --user james
```
