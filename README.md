FluxCD with EKS:
----------------
Pre-requsite:
-------------
Eks cluster
Git
FluxCD
FluxCTL

Install fluxCTL:
---------------
    curl -L https://github.com/fluxcd/flux/releases/latest/download/fluxctl_linux_amd64 -o fluxctl 
    mv fluxctl_linux_amd64 fluxctl
    chmod +x fluxctl
    mv fluxctl /usr/bin/
    fluxctl version

Create a namespace:
-------------------
kubectl create ns springboot

Then, install Flux in your cluster (replace YOURUSER with your GitHub username):
--------------------------------------------------------------------------------
    export GHUSER="Abhi-chintu"
    fluxctl install \
    --git-user=${GHUSER} \
    --git-email=${GHUSER}@users.noreply.github.com \
    --git-url=git@github.com:${GHUSER}/gitops_fluxcd \
    --git-branch=master \
    --git-path=kubernetes \
    --namespace=springboot | kubectl apply -f -


 Edit Deployment file
  
    kubectl edit deploy flux -n springboot
    
    Add "--sync-garbage-collection"
    -----------------------------------
    - args:
      - --memcached-service=
      - --sync-garbage-collection=true    
    -----------------------------------

After installing the flux check whether its been installed or not:
-----------------------------------------------------------------
kubectl get deploy -n springboot

Authorise Flux CD to Connect to Your Git Repository:
We now need to allow the Flux CD operator to interact with the Git repository, and therefore, we need to add its public SSH key to the repo.
Get the public SSH key using fluxctl:

--> fluxctl identity --k8s-fwd-ns springboot

Check if Flux deployment is successful or not:
---------------------------------------------
--> kubectl get deploy -n springboot

Authorise Flux CD to Connect to Your Git Repository:
----------------------------------------------------
We now need to allow the Flux CD operator to interact with the Git repository, and therefore, we need to add its public SSH key to the repo.
Get the public SSH key using fluxctl:

--> fluxctl identity --k8s-fwd-ns springboot

In order to sync your cluster state with git you need to copy the public key and create a deploy key with write access on your GitHub repository. 
Open GitHub Repo --> settings --> Deploy Keys Add public key inside Deploy Keys, 
Click on Allow write access check box and then click on Add Key

By default, Flux git pull frequency is set to 5 minutes. You can tell Flux to sync the changes immediately with:
----------------------------------------------------------------------------------------------------------------
--> fluxctl sync --k8s-fwd-ns springboot


