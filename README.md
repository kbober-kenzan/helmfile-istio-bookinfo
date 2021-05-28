# BookInfo Helm charts with HelmFile release and GIT Helm repository

This readme will provide steps for the following:

- How to utilize a GIT repository as a Helm chart repository.
- Package and publish Helm charts to GIT repository.
  - <helm-package>.tgz
  - index.yaml
- Create individaul Helm charts for each of the components that comrpise of the Istio BookInfo sample application
  - gateway components
  - service components
  - microservice deployment components
- Create a Helmfile to release the Istio BookInfo application utilizing the individually versioned Helm chart components located in a Helm GIT repository

The Istio BookInfo Helm charts were derived from the following GIT repository origin:
[git@github.com:evry-ace/helm-istio-bookinfo.git](https://github.com/evry-ace/helm-istio-bookinfo)

## Utilize a GIT repository as Helm chart repository

To utilize a GIT repository as a Helm chart repository the following Helm plugin must be installed:

```http
  https://github.com/diwakar-s-maurya/helm-git

  helm plugin install https://github.com/diwakar-s-maurya/helm-git
```

Create a GIT repository on the GIT platform of your choice.
For this example we will use a public GitHub and private Gitlab repository

- GitHub - https://github.com/kbober-kenzan/istio-bookinfo-helmfile
- GitLab - https://github.com/kbober-kenzan/istio-bookinfo-helmfile




## Package and publish Helm charts to GIT Repository

## Integrate Helm chart repository with Helmfile

- Add the following snippet to the Helmfile Repository element:

``` yaml
repositories:
  - name: github-kbober-kenzan
    url: github://kbober-kenzan/istio-bookinfo-helmfile:main/helm-repo
  - name: gitlab-kbober-kenzan
    url: gitlab://kbober-kenzan/istio-bookinfo-helmfile:main/helm-repo 
```

## Publish the Helmfile to Kubernetes instance

- Execute the following command to create/update the 'Helmfile.lock'

```bash
  helmfile deps
```

- Publish/apply the BookInfo Helmfile against a Kubernetes instance

```bash
  helmfile apply
```

- Verify that that Helm charts were deployed

```bash
  helm chart list
```

- Verify the BookInfo application was successfully deployed

```bash
  kubectl exec "$(kubectl get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}')" -c ratings -- curl -sS productpage:9080/productpage | grep -o "<title>.*</title>"
```

## Useful Links

- [Helm](https://helm.sh/)
- [Helmfile](https://github.com/roboll/helmfile)
- [Helm Plugin: helm-git](https://github.com/diwakar-s-maurya/helm-git)