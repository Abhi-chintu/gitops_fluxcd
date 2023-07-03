**Gitops Fluxcd on EKS Cluster**

**Pre Requisites**
  Eks cluster
  GIT 
  Docker 
  FluxCD

**Install fluxCTL**
  curl -L https://github.com/fluxcd/flux/releases/latest/download/fluxctl_linux_amd64 -o fluxctl
  mv fluxctl_linux_amd64 fluxctl
  chmod +x fluxctl
  mv fluxctl /usr/bin/
  fluxctl version

**Create a namespace**
  kubectl create ns springboot

**Then, install Flux in your cluster (replace YOURUSER with your GitHub username): Syntax**
export GHUSER="<Github-username>"
fluxctl install \
--git-user=${GHUSER} \
--git-email=${GHUSER}@users.noreply.github.com \
--git-url=git@github.com:${GHUSER}/<repo-name>\
--git-branch=<branch-name> \
--git-path=<path-name> \
--namespace=<namespace-name> | kubectl apply -f -

export GHUSER="Abhi-chintu"
fluxctl install \
--git-user=${GHUSER} \
--git-email=${GHUSER}@users.noreply.github.com \
--git-url=git@github.com:${GHUSER}/gitops_fluxcd \
--git-branch=master \
--git-path=kubernetes \
--namespace=springboot | kubectl apply -f -

**After installing the flux check whether its been installed or not**
kubectl get deploy -n springboot

**Edit Deployment file**

kubectl edit deploy flux -n springboot

Add "--sync-garbage-collection"
- args:
  - --memcached-service=
  - --sync-garbage-collection=true    







