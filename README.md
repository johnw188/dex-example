# dex-example
Example kubernetes config files for configuring dex to tie into active directory, built off of the examples found at https://github.com/coreos/dex/tree/master/contrib/k8s. The login app is in dex-example-app, and is very bare bones, you have to paste in your client ID to start the auth process.

Configure your kube api server with the following flags (see https://github.com/coreos/dex/tree/master/contrib/k8s for more info):

```
    - --oidc-issuer-url=https://dex.[your domain here].com %>
    - --oidc-client-id=[client ID returned when configuring dex]
    - --oidc-username-claim=email [this overrides the field that kube considers a username in the returned oidc token]
```

Once you've logged in dex will return a token, paste it into your `~/.kube/config` as shown below.

```
apiVersion: v1
clusters:
- cluster:
    server: [your api server here]
  name: cluster-name
contexts:
- context:
    cluster: cluster-name
    namespace: kube-system
    user: kube-user
  name: pdxtst
current-context: cluster-name
kind: Config
preferences: {}
users:
- name: kube-user
  user:
    token: [paste the full token from dex here]
```
