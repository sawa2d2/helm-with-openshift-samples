# Jenkins

## Prerequisites
- OKD4 (v4.14.0)
  - StorageClass that be able to dynamic provisioning. (e.g. `ocs-storagecluster-ceph-rbd` of ODF)
- `helm`
- `yq`

## Steps
https://artifacthub.io/packages/helm/jenkinsci/jenkins

```
# Add repository
$ helm repo add jenkinsci https://charts.jenkins.io/

# Update repository
$ helm repo update

# Install jenkins
$ helm install jenkins jenkinsci/jenkins --version 5.0.10 -f values.yaml

# Create route
$ oc create route edge --service=jenkins --hostname=jenkins.apps.ocp4.example.com --port='8080' --insecure-policy='Redirect'
```

FYI: If a pod fails due to insufficient resources of the cluster, edit StatefulSet by `oc edit sts jenkins` then replace `resources` keys to `resources: {}`.

Check if pods are running:
```
$ oc get pods
NAME        READY   STATUS    RESTARTS   AGE
jenkins-0   2/2     Running   0          4m43s
```

Extract credentials of Jenkins admin user:
```
$ kubectl get secrets jenkins -o yaml | yq '.data.jenkins-admin-user' | base64 --decode
admin
$ kubectl get secrets jenkins -o yaml | yq '.data.jenkins-admin-password' | base64 --decode
<decoded_password>
```

Then access and login jenkins from `https://jenkins.apps.ocp4.example.com`.

## Cleanup
```
$ helm uninstall jenkins
$ oc delete route jenkins
```

## Troubleshooting Tips

To check default values of helm chart, execute as follows:
```
$ helm show values jenkins/jenkins > jenkins.yaml
```

To check init process log, execute as follows:
```
$ oc logs jenkins-0 -c init 
```

## GitHub issue references
- [Permission denied on disable Setup Wizard step · Issue #210 · jenkinsci/helm-charts](https://github.com/jenkinsci/helm-charts/issues/210)
- [(k8s v1.25 bug?) compute-resources: must specify limits.cpu for: config-reload; limits.memory for: config-reload; requests.cpu for: config-reload; requests.memory for: config-reload · Issue #737 · jenkinsci/helm-charts](https://github.com/jenkinsci/helm-charts/issues/737)
- [Error while deploying on OpenShift: AccessDeniedException: /.cache · Issue #506 · jenkinsci/helm-charts](https://github.com/jenkinsci/helm-charts/issues/506#issuecomment-1050898826)


