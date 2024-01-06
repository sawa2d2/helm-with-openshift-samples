## StackEdit
手順は https://github.com/benweet/stackedit?tab=readme-ov-file#deploy-with-helm に従う。

```
git clone git@github.com:benweet/stackedit.git

# Add the StackEdit Helm repository
helm repo add stackedit https://benweet.github.io/stackedit-charts/

# Update your local Helm chart repository cache
helm repo update

# Deploy StackEdit chart to your cluster
helm install --name stackedit stackedit/stackedit \
  --set dropboxAppKey=$DROPBOX_API_KEY \
  --set dropboxAppKeyFull=$DROPBOX_FULL_ACCESS_API_KEY \
  --set googleClientId=$GOOGLE_CLIENT_ID \
  --set googleApiKey=$GOOGLE_API_KEY \
  --set githubClientId=$GITHUB_CLIENT_ID \
  --set githubClientSecret=$GITHUB_CLIENT_SECRET \
  --set wordpressClientId=\"$WORDPRESS_CLIENT_ID\" \
  --set wordpressSecret=$WORDPRESS_CLIENT_SECRET
```
