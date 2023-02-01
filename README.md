# Helm-Chart-Basics-Getting-Started-Hands-on
Helm Chart Basics-Getting Started-Hands-on

## Install helm on windows

```
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh
```

## Create the skeleton/first chart

```
helm create webapp1
```

```
rm -rf webapp1/templates/*
```

## Start minikube and sync docker-env

```
minikube start
eval $(minikube docker-env)
```
## Start linter to check syntax errors before deployment

`helm lint ./webapp1/`

![image](https://user-images.githubusercontent.com/96833570/216052162-2042a3d8-e658-47ab-9fd6-6e70a341e81e.png)


## Replacing templates dir and deploy the first helm chart

Copy and paste deployment and svc files in themplates directory and run:

`helm install wywebapp-release webapp1/`


### alternative

```
kubectl create deployment nginx --image=nginx --dry-run=client -o yaml >> webapp1/templates/nginx.yaml
kubectl create deployment nginx --image=nginx
kubectl expose deploy nginx --port 80 --type NodePort --dry-run=client -o yaml >> webapp1/templates/service.yaml
```

## Templating

![image](https://user-images.githubusercontent.com/96833570/216051343-7ecef42f-c08c-48cb-af44-133e1d3af250.png)

![image](https://user-images.githubusercontent.com/96833570/216051531-9206ffc7-bb87-4c3d-81e1-06b3084008c9.png)

![image](https://user-images.githubusercontent.com/96833570/216051689-2c1edefa-9c71-460f-8731-c91d0a6c2a40.png)

## Dispaly info to the user

Add the following to the NOTES.txt file. 

```
servicename=$(k get service -l "app={{ .Values.appName }}" -o jsonpath="{.items[0].metadata.name}")
kubectl --namespace {{ .Values.namespace}} port-forward service/{{ .Values.appName }} 8888:80
```

`helm upgrade wywebapp-release webapp1/ --values webapp1/values.yaml`

`kubectl --namespace default port-forward service/myhelmapp 8888:80`

![image](https://user-images.githubusercontent.com/96833570/216103208-ee52f2df-d7e5-4382-b9cc-fcdf8222e1c9.png)

![image](https://user-images.githubusercontent.com/96833570/216103923-c448b42e-65ba-4a89-888e-6e1b9314a7c1.png)

## helm list

Display all releases for a specified namespace: `helm list`

![image](https://user-images.githubusercontent.com/96833570/216104849-678704a8-1b26-46be-a682-be878b601dc8.png)




## helm package ./wywebapp

Package your manifests: `helm package ./webapp1`

![image](https://user-images.githubusercontent.com/96833570/216105548-0d403618-1e5f-4e9f-adf1-d09150856fd6.png)


## helm uninstall 

Uninstall the release and remove all of the resources associated with the last release of the chart: `helm uninstall wywebapp-release`

![image](https://user-images.githubusercontent.com/96833570/216105850-2dbd6375-7259-4a0a-bb26-dc821fb932fb.png)







