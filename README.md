# sample-api
*Deploying An Application On Kubernetes

*Dockerize The Application
- go mod init github.com/denis-kravchenko/sample-api
- go build

*Creating A Deployment
- git add deployment.yml
- git commit  -m "Adds the Deployment file"
- git push

*Exposing Our Application Using Service And Ingress

*Installing Nginx Ingress Controller
- kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/nginx-0.30.0/deploy/static/mandatory.yaml
- kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/nginx-0.30.0/deploy/static/provider/cloud-generic.yaml

*Handling Application Configuration Using ConfigMaps
- kubectl create configmap config.json --from-file=config.json

*Securing Confidential Data Using Secrets
- kubectl create secret generic redis-password --from-literal=redis-password=password123

*Deploying The Backend Storage (Redis) Using A StatefulSet

*Adding HTML Content To The Application

*Packaging Our Kubernetes Cluster Using Helm
- kubectl -n kube-system create serviceaccount tiller
- kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
- helm init --service-account tiller

*Now that we have our files ready, execute the following commands to destroy our environment. We are going to use Helm to rebuild it automatically:
- kubectl delete deployments frontend static
- kubectl delete statefulsets redis
- kubectl delete svc frontend-svc redis-svc static-svc
- kubectl delete configmaps app-config
- kubectl delete ing frontend-ingress

*To redeploy our application stack (including the frontend, static and the backend Redis instance), we use only one command:
- helm install --name messagesapp-helm helm/

*For example, we may need to make changes to our ingress resource template. To apply this change, you just run a command like this:
-helm upgrade messagesapp-helm helm/

*Finally, you can use Helm to destroy the environment and remove all the resources that it created
- helm delete messagesapp-helm
