# kubectl-cheatsheet

## kubectx - change context
```
# check context
kubectx
## output
#k191011
#k191119

# switch context
kubectx k191119
## output
# Switched to context "k191119".
```

## kubens - change namespace
```
# check namespaces
kubens
## output
# cert-manager
# default
# downscaler
# ingress-nginx
# kube-node-lease
# kube-public
# kube-system
# kubernetes-dashboard

# change namespace
kubens kube-system
## output
# Context "k191119" modified.
# Active namespace is "kube-system".
```

## get Node and Pod
Reference: https://stackoverflow.com/questions/48983354/kubernetes-list-all-pods-and-its-nodes
```
kubectl get pod -o=custom-columns=NODE:.spec.nodeName,IP:.status.podIP,NAMESPACE:.metadata.namespace,NAME:.metadata.name,PHASE:.status.phase --all-namespaces

NODE                                               NAMESPACE              NAME                                             PHASE
ip-172-26-36-212.ap-southeast-2.compute.internal   cert-manager           cert-manager-74867db95f-lwjqs                    Running
ip-172-26-36-212.ap-southeast-2.compute.internal   cert-manager           cert-manager-cainjector-5c86c7b87b-7bzvn         Running
ip-172-26-36-212.ap-southeast-2.compute.internal   cert-manager           cert-manager-webhook-89bc6bfcc-c79xt             Running
ip-172-26-36-212.ap-southeast-2.compute.internal   downscaler             kube-downscaler-667c47bf86-bv4rc                 Running
ip-172-26-36-212.ap-southeast-2.compute.internal   ingress-nginx          nginx-ingress-controller-776b444cd4-7d8f9        Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kube-system            aws-node-rnxrz                                   Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kube-system            cluster-autoscaler-59f9f4fbc6-tvbsp              Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kube-system            coredns-6c4c65cc7f-6bgrm                         Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kube-system            coredns-6c4c65cc7f-v574r                         Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kube-system            k8s-spot-termination-handler-tr4ld               Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kube-system            kube-proxy-nkh55                                 Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kube-system            metrics-server-565b4cf7ff-h6gl7                  Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kube-system            registry-creds-ap-southeast-2-7b6958dd56-kqgpk   Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kube-system            registry-creds-us-west-2-7c554d8df4-kbl7p        Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kubernetes-dashboard   dashboard-metrics-scraper-5f6fd8f7cc-l7xgf       Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kubernetes-dashboard   kubernetes-dashboard-6dcffbfb96-2sn7g            Running

# no header
kubectl get pod -o=custom-columns=NODE:.spec.nodeName,IP:.status.podIP,NAMESPACE:.metadata.namespace,NAME:.metadata.name,PHASE:.status.phase --all-namespaces --no-headers=true

ip-172-26-36-212.ap-southeast-2.compute.internal   cert-manager           cert-manager-74867db95f-lwjqs                    Running
ip-172-26-36-212.ap-southeast-2.compute.internal   cert-manager           cert-manager-cainjector-5c86c7b87b-7bzvn         Running
ip-172-26-36-212.ap-southeast-2.compute.internal   cert-manager           cert-manager-webhook-89bc6bfcc-c79xt             Running
ip-172-26-36-212.ap-southeast-2.compute.internal   downscaler             kube-downscaler-667c47bf86-bv4rc                 Running
ip-172-26-36-212.ap-southeast-2.compute.internal   ingress-nginx          nginx-ingress-controller-776b444cd4-7d8f9        Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kube-system            aws-node-rnxrz                                   Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kube-system            cluster-autoscaler-59f9f4fbc6-tvbsp              Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kube-system            coredns-6c4c65cc7f-6bgrm                         Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kube-system            coredns-6c4c65cc7f-v574r                         Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kube-system            k8s-spot-termination-handler-tr4ld               Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kube-system            kube-proxy-nkh55                                 Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kube-system            metrics-server-565b4cf7ff-h6gl7                  Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kube-system            registry-creds-ap-southeast-2-7b6958dd56-kqgpk   Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kube-system            registry-creds-us-west-2-7c554d8df4-kbl7p        Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kubernetes-dashboard   dashboard-metrics-scraper-5f6fd8f7cc-l7xgf       Running
ip-172-26-36-212.ap-southeast-2.compute.internal   kubernetes-dashboard   kubernetes-dashboard-6dcffbfb96-2sn7g            Running

# Pod status != Running
kubectl get pod -o=custom-columns=NODE:.spec.nodeName,IP:.status.podIP,NAMESPACE:.metadata.namespace,NAME:.metadata.name,PHASE:.status.phase --all-namespaces --no-headers=true --field-selector=status.phase!=Running

# Delete not Running pods filter by a namespace
# Remove "grep xxx" to delete all not Running pods
kubectl get pod -o=custom-columns=NODE:.spec.nodeName,IP:.status.podIP,NAMESPACE:.metadata.namespace,NAME:.metadata.name,PHASE:.status.phase --all-namespaces --no-headers=true --field-selector=status.phase!=Running  | grep jz8 | xargs -n5 bash -c 'kubectl delete pod "$3" -n "$2"'

```

sort and filter
```
# filter by IP
...| grep -E -- '141-32|46-84|93-137'

# sort by node, then namespace
... | sort -k1,1 -k2,2

# count
... | wc -l

```

## All namespaces with pod

```
kubectl get pod -o=custom-columns=NAMESPACE:.metadata.namespace --all-namespaces --no-headers=true  | awk '{print $1}' | uniq
```

TODO:
- all namespace + those with and without pod
- all namespace with certain annotation


## Patch
Reference: https://kubernetes.io/docs/reference/kubectl/cheatsheet/#patching-resources
```
# apply a patch
kubectl patch deployment coredns --patch "$(cat $module_path/main/patch-tolerations.yaml)" -n kube-system

# remove a patch
kubectl patch deployment valid-deployment  --type json   -p='[{"op": "remove", "path": "/spec/template/spec/containers/0/livenessProbe"}]'

```

## Drain and delete node
```
# separate command
kubectl get node
kubectl drain <node name> --ignore-daemonsets --force --delete-local-data
kubectl delete node ip-172-26-16-138.ap-southeast-2.compute.internal

# in one step
kubectl get node --no-headers -o custom-columns=":metadata.name" | xargs -n1 kubectl drain --ignore-daemonsets --force --delete-local-data
kubectl get node --no-headers -o custom-columns=":metadata.name" | xargs -n1 kubectl delete node 
```


## namespaces
```
# get all namespace filtered by and output command list
kubectl get ns -o "jsonpath={range .items[?(@.metadata.labels.engineer)]}kubob ns create {@.metadata.name} {@.metadata.labels.engineer}{ '\n' }{end}"

# get all namespace with label engineer exists and apply yaml to it
kubectl get namespace --no-headers -l engineer -o custom-columns=":metadata.name" | xargs -n1 kubectl apply --recursive -f services/core/ -n
```

**get node's purchase type and ip**
```
kubectl get node -o "jsonpath={range .items[?(@.kind)]}{@.metadata.labels.node-purchase-type},{@.status.addresses[0].address}{ '\n' }{end}" | sort -t, -k1,1 -k2,2

```