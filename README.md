# playground

kubectl create deployment hello-app --image=gcr.io/google-samples/hello-app:2.0
kubectl expose deployment hello-app --name=hello-app-service --type=NodePort --port 8080 --target-port 8080
kubectl wait --for=condition=ready pod -l app=hello-app --timeout=120s
curl http://$(minikube ip):$(kubectl get svc hello-app-service -o json | jq -r '.spec.ports[0].nodePort')

kubectl get svc hello-app-service -o yaml > service.yaml
kubectl get deployment hello-app -o yaml > deployment.yaml

find yaml-files/ -type f -name "*.yaml" -exec kubectl apply -f {} \;

helm package helm-chart/my-hello-app/
helm upgrade --install my-hello-app my-hello-app-1.0.0-0.tgz
curl http://$(minikube ip):$(kubectl get svc my-hello-app-service -o json | jq -r '.spec.ports[0].nodePort')

