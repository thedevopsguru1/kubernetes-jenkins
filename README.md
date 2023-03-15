# Kubernetes Manifests for Jenkins Deployment

Refer https://devopscube.com/setup-jenkins-on-kubernetes-cluster/ for step by step process to use these manifests.

##########HELM ########

New way using helm chart:
```
helm repo add jenkinsci https://charts.jenkins.io/
```
```
helm repo update
```
```
kubectl create ns jenkins
```
```
helm install my-jenkins jenkinsci/jenkins -n jenkins -f helm_values.yaml
```

helm link: https://artifacthub.io/packages/helm/jenkinsci/jenkins

### Add the ingress: ingress.yaml
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
 name: jenkins-ingress
 namespace: jenkins
 annotations:
   cert-manager.io/cluster-issuer: "letsencrypt-prod"
   kubernetes.io/ingress.class: "nginx"
   kubernetes.io/tls-acme: "true"
   #nginx.ingress.kubernetes.io/ssl-passthrough: "true"
   # If you encounter a redirect loop or are getting a 307 response code
   # then you need to force the nginx ingress to connect to the backend using HTTPS.
   #
  # nginx.ingress.kubernetes.io/backend-protocol: "HTTPS"
spec:
 rules:
 - host: jenkins.anaeleboo.com
   http:
     paths:
     - path: /
       pathType: Prefix
       backend:
         service:
           name: my-jenkins
           port:
             name: http
 tls:
 - hosts:
   - jenkins.anaeleboo.com
   secretName: jenkins-secret # do not change, this is provided by Argo CD
```

#####################################################################################################



#######################################################################################################

https://www.youtube.com/watch?v=bcjOOp7x7i0&t=110s
