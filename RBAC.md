# CKA

## RBAC - Create User, Certificates. Role & Rolebindings

#### 1. Create cerficate and key pairs for user kubetest

     # Create namespace
     kubectl create ns sysadmin

     # Generate private key for user kubetest
     openssl genrsa -out kubetest.key 2048
     
     # Create Certificare Signing Request using private key - kubetest.key
     openssl req -new -key kubetest.key -out kubetest.csr -subj "/CN=kubetest/O=sysadmin"
     
     # Sign kubetest.csr by Certficate Authority
     sudo openssl x509 -req -in kubetest.csr -CA  /etc/kubernetes/pki/ca.crt -CAkey  /etc/kubernetes/pki/ca.key -CAcreateserial -out kubetest.crt -days 365
     
     
 #### 2. Create kubeconfig for user kubetest
 
     # create a .kubeconfig for user kubetest using main kubeconfig file
     
     cp ~/.kube/config kubetest.kubeconfig
     
     # Get base64 encoded value of the certificate
     cat kubetest.crt | base64 -w0
     
     # Get base64 encoded value of the key
     cat kubetest.key | base64 -w0
     
     # Update kubetest.kubeconfig as highlighted
     ![image](https://user-images.githubusercontent.com/48376100/132819656-6238bf66-4754-4143-8d43-df6884baa9d6.png)


#### 3. Create namepsace Role and Role Bindings
    
     
    # Create Role
     kubectl create role kubetest-sysadmin --verb=list,get --resource=pods --namespace=sysadmin
     
    # Create Rolebidning
     kubectl create rolebinding kubetest-sysadmin-rolebinding --role=kubetest-sysadmin --user=kubetest --namespace=sysadmin
