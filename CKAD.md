# Certified Kubernetes Application Developer Notes

## Important Commands

```     
kubectl run nginx -- image=nginx --restart=Never --dry-run -o yaml > mypod.yaml
```

## Section I: Core Concepts

## Section II: Configuration

Configuration Maps: allows for central management of configuration data that you would put into the pod definition file and use it in multiple pods as needed. Passes configuration data in key value pairs.

Utilizes the <b> envFrom </b>  command

* How to configure (Imperative vs Declarative)
Imparative: without using a definition file; uses CLI and has two ways of passing in
- Literal
```
kubectl create configMap NameOfConfigMap --from-literal=Key_Value=Pair --from-literal SecondKeyValue=Pair
```
- File
```
> kubectl create configMap NameOfConfigMap --from-file=PathOfFile 

Example: kubectl create configMap NameOfConfigMap --from-app_config.properties
```
Declarative: using a configMap definition file
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  app_color: blue
  app_mode: prod
```
* How to inject into pod (ConfigMap File vs Single Env vs Volume)
Note: pass in the "METADATA" name from previously created ConfigMap

Option 1: ConfigMap File allows you to pass in all params
```
spec:
  containers:
    envFrom:
      - configMapRef:
           name: <METADATA.name>
```

Option 2: SingleEnv allows you to pull out what exactly you need from ConfigMapFIle
```
spec:  
  containers:
  - image: nginx
    name: nginx
    env:
      - name: Unique_App_Key
        valueFrom:
          configMapKeyRef: 
            name: app-config
            key: key123
```

Option 3: Attached a Volume to the Pod
```
volumes:
- name: app-config-volume
  configMap:
    name: app-config
```

## Section X: Secrets
