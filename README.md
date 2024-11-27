# FECP4-1022-Lab-1-Deploying-environments-in-Kubernetes

## Create prod environment and configurations
``` kubectl create namespace production ```

## PROD CONFIG
``` kubectl create configmap app-config --from-literal=APP_ENV=production --namespace=production ```

## PROD SECRETS
``` kubectl create secret generic app-secret --from-literal=DB_PASSWORD=prodpassword123 --namespace=production ```

## View secrets
``` kubectl get secrets -n production ```

## Describe Secrets
``` kubectl describe secret app-secret -n production  ```

## Deploy to Prod
```
sed -e 's/PLACEHOLDER_NAMESPACE/production/' \
    -e 's/replicas: .*$/replicas: 3/' staging-deployment.yaml > production-deployment.yaml

kubectl apply -f production-deployment.yaml
```

## Expose Prod environment
```
kubectl expose production nginx-app \
  --type=NodePort \
  --name=nginx-service \
  --port=80 \
  --target-port=80 \
  --namespace=production
```

## Verify deployment
```
kubectl get pods -n production
kubectl get service -n production


kubectl exec -it nginx-app-65968468b6-5lz78 -n production -- /bin/bash
echo $APP_ENV
echo $DB_PASSWORD
```
