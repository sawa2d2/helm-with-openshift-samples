# Pull-Jenkins

## Prerequisites
- `helm`

## Steps
手順は https://artifacthub.io/packages/helm/jenkinsci/jenkins に従う。

```
$ helm repo add jenkinsci https://charts.jenkins.io/
$ helm repo update
```

Release 名を調べる
```
$ helm search repo jenkins
NAME            CHART VERSION   APP VERSION     DESCRIPTION                                       
jenkins/jenkins 4.11.2          2.426.2         Jenkins - Build great things at any scale! The ...
```

Jenkins を deploy する
```
$ helm install jenkins jenkinsci/jenkins --version 4.11.2 --set persistence.enabled=false

NAME: jenkins
LAST DEPLOYED: Fri Jan  5 07:34:10 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
1. Get your 'admin' user password by running:
  kubectl exec --namespace default -it svc/jenkins -c jenkins -- /bin/cat /run/secrets/additional/chart-admin-password && echo
2. Get the Jenkins URL to visit by running these commands in the same shell:
  echo http://127.0.0.1:8080
  kubectl --namespace default port-forward svc/jenkins 8080:8080

3. Login with the password from step 1 and the username: admin
4. Configure security realm and authorization strategy
5. Use Jenkins Configuration as Code by specifying configScripts in your values.yaml file, see documentation: http://127.0.0.1:8080/configuration-as-code and examples: https://github.com/jenkinsci/configuration-as-code-plugin/tree/master/demos

For more information on running Jenkins on Kubernetes, visit:
https://cloud.google.com/solutions/jenkins-on-container-engine

For more information about Jenkins Configuration as Code, visit:
https://jenkins.io/projects/jcasc/


NOTE: Consider using a custom image with pre-installed plugins
```

```
$ kubectl get pods
NAME        READY   STATUS    RESTARTS   AGE
jenkins-0   0/2     Pending   0          45s
```

```
kubectl get secrets jenkins -o yaml
apiVersion: v1
data:
  jenkins-admin-password: <admin_password>
  jenkins-admin-user: YWRtaW4=
```

秘匿情報を base64 でデコードして確認する。
```
$ kubectl get secrets jenkins -o yaml | yq '.data.jenkins-admin-password' | base64 --decode
<decoded_password>

$ kubectl get secrets jenkins -o yaml | yq '.data.jenkins-admin-user' | base64 --decode
admin
```
