# Setup Datadog in a k8s cluster 

# Apply rules for RBAC
kubectl apply -f cluster-agent-rbac.yaml

#Apply RBAC
kubectl apply -f rbac.yaml

#Create a secret
kubectl create secret generic datadog-secret --from-literal api-key="<YOUR_API_KEY>"


#create a password to encode which should be 32 char long
https://www.passwordpal.net/password-generator.php
pass= vn9Tamc~rmU/ncCZ$udk%:~;Z__iMTp5

#create a secret token to enable secure Agent-to-Agent communication between the Cluster Agent and the node-based Agents
echo -n '<32_CHARACTER_LONG_STRING>' | base64
encoded password = dm45VGFtY35ybVUvbmNDWiR1ZGslOn47Wl9faU1UcDU=

#Use the resulting token to create a Kubernetes secret that both flavors of Agent will use to authenticate with each other
kubectl create secret generic datadog-auth-token --from-literal=token=<TOKEN_FROM_PREVIOUS_STEP>  #use encoded password

#Deploy datadog cluster agent
 kubectl apply -f datadog-cluster-agent.yaml

#Check that the Cluster Agent deployed successfully
kubectl get pods -l app=datadog-cluster-agent



#### Now we have to deploy a node based agent ###
kubectl apply -f datadog-agent.yaml

#check if deployment is sucessful
kubectl get pods -l app=datadog-agent

