# kubectl delete secret  aws -n  argo-events
# kubectl create secret generic aws \
#  --from-literal=accessKey= \
#  --from-literal=secretKey= \
#  -n argo-events
kubectl delete  -n argo-events -f 'c:/Users/TAAEBAB1/argo-workflows/event/trigger_sources/sensor-git.yaml'
kubectl apply -n argo-events -f 'c:/Users/TAAEBAB1/argo-workflows/event/trigger_sources/sensor-git.yaml'
curl -d '{"message":"ok"}' -H "Content-Type: application/json" -X POST http://localhost:12000/example

#!/bin/bash

# Get the current time
now=$(date +%s)

# Get all pods in the argo-events namespace
pods=$(kubectl get pods -n argo-events -o jsonpath='{.items[*].metadata.name}')

# Loop over all pods and print their logs
for pod in $pods; do
  echo "Logs for $pod:"
  kubectl logs --since=1m $pod -n argo-events
done

# Get all events in the argo-events namespace from the last 1 minute
echo "Events in argo-events namespace from the last 1 minute:"
kubectl get events --field-selector=lastTimestamp=$(date -d'1 minute ago' -u +'%Y-%m-%dT%H:%M:%SZ') -n argo-events

kubectl -n argo-events get wf