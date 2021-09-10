# CKA

## RBAC - Create User, Certificates. Role & Rolebindings

#### 1. Create cerficate and key pairs for user kubetest

     # Generate private key for user kubetest
     openssl genrsa -out kubetest.key 2048
     
     # Create Certificare Signing Request using private key - kubetest.key
     openssl req -new -key kubetest.key -out kubetest.csr -subj "/CN=kubetest/O=sysadmin"
     
     # Sign kubetest.csr by Certficate Authority
     sudo openssl x509 -req -in kubetest.csr -CA  /etc/kubernetes/pki/ca.crt -CAkey  /etc/kubernetes/pki/ca.key -CAcreateserial -out kubetest.crt -days 365

