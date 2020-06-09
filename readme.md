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

run pod / deployment from yaml:\
`kubectl apply --filename private_deploy.yaml`

forward or expose pod:\
`kubectl port-forward repo1 8080:80`\
`kubectl expose pod repo1 --type="NodePort"`

expose deployment:\
`kubectl expose deployment dep-repo1 --type=LoadBalancer`

nodeport in:\
`kubectl get svc`
`kubectl describe service repo1`

exposed on:\
(minikube ip):&nodePort
