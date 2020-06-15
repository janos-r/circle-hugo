## docker

build image:\
`docker build -t radimj/repo1 .`

run and forward port to localhost:\
`docker run -d -p 80:80 radimj/repo1`

push to docker hub:\
`docker push radimj/repo1`

## kubernetes

create secret from docker login:

```
kubectl create secret generic regcred \
    --from-file=.dockerconfigjson=<path/to/.docker/config.json> \
    --type=kubernetes.io/dockerconfigjson
```

run once pod with secret:\
`kubectl run repo1 --overrides='{ "spec": { "imagePullSecrets": [{"name": "regcred"}] } }' --image=radimj/repo1 --port=80`

forward or expose pod:\
`kubectl port-forward repo1 8080:80`\
`kubectl expose pod repo1 --type="NodePort"`

expose deployment:\
`kubectl expose deployment circle-deployment --type=LoadBalancer`

nodeport in:\
`kubectl get svc`
`kubectl describe service repo1`

exposed on:\
(minikube ip):&nodePort

create new yaml file from:\
`kubectl get (deploy / svc / pod ) -o yaml`

run pod / deployment from yaml:\
`kubectl apply --filename private_deploy.yaml`

revert (delete) from yaml:\
`kubectl delete -f private_deploy.yaml`

## helm

display docker login secret from kubectl:\

```
kubectl get secret regcred \
    --output jsonpath="{.data.\.dockerconfigjson}" | \
    base64 --decode | \
    jq ".auths | map(.auth)[0]" -r | \
    base64 --decode
```

lint chart:\
`helm lint circle-chart/`

build package:\
`helm package circle-chart/`

install: (name should be same as package name)\
`helm install circle circle-0.2.0.tgz`

check release:\
`helm ls`

uninstall totally:\
`helm uninstall circle`

# todo:
rolling update
