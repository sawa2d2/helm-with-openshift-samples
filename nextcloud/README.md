## Nextcloud

手順は https://artifacthub.io/packages/helm/nextcloud/nextcloud に従う。

helm repo add nextcloud https://nextcloud.github.io/helm/
helm install my-nextcloud nextcloud/nextcloud --version 4.5.10
