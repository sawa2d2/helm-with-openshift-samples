# Jupyter Hub

## Steps
手順は https://artifacthub.io/packages/helm/jupyterhub/jupyterhub に従う。

```
$ helm repo add jupyterhub https://jupyterhub.github.io/helm-chart/

$ helm repo update

$ helm install jupyterhub jupyterhub/jupyterhub --version 3.2.2-0.dev.git.6511.h37376786 -f values.yaml
```

## Troubleshooting Tips

To check default values of helm chart, execute as follows:
```
$ helm show values jupyterhub/jupyterhub > jupyterhub.yaml
```

To check init process log, execute as follows:
```
$ oc logs <pod> -c init
```

## Links
- [jupyterhub/zero-to-jupyterhub-k8s: Helm Chart & Documentation for deploying JupyterHub on Kubernetes](https://github.com/jupyterhub/zero-to-jupyterhub-k8s)
