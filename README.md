# Tektoncd-operator

## Dev env

### Prerequisites

1. operator-sdk: https://github.com/operator-framework/operator-sdk

2. minikube: https://kubernetes.io/docs/tasks/tools/install-minikube/

### Setup Minikube

**create minikube instance**

```
minikube start -p mk-tekton \
 --cpus=2 --memory=6144 --kubernetes-version=v1.12.0 \
 --extra-config=apiserver.enable-admission-plugins="LimitRanger,NamespaceExists,NamespaceLifecycle,ResourceQuota,ServiceAccount,DefaultStorageClass,MutatingAdmissionWebhook"  \
 --extra-config=apiserver.service-node-port-range=80-32767
```

**set docker env**

```
eval $(minikube docker-env -p mk-tekton)
```

### Install OLM

**clone OLM repository (into go path)**

```
git clone git@github.com:operator-framework/operator-lifecycle-manager.git \
          $GOPATH/github.com/operator-framework/
```

```
kubectl apply -f $GOPATH/github.com/operator-framework/operator-lifecycle-manager/deploy/upstream/quickstart
```

### Deploy tekton-operator

#### On minikube for testing

1. Apply operator crd

```
kubectl apply deploy/crds/*_crd.yaml
```


2. Apply operator namespace, operatorgroup, serviceaccount, role, and rolebinding

```
kubectl -n tekton-pipelines apply -f deploy/
```


3. apply the `olm-catalog`

```
  kubectl -n tekton-pipelines apply -f deploy/olm-catalog/tektoncd-operator/0.0.1/
```

4. once an instance of the cr is created the tekton-operator will launch a pod
```
kubectl apply deploy/crds/*_cr.yaml
```
