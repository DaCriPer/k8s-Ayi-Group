# Sección 6: Pods

1. Iniciar minikube 

```bash
minikube start
```

2. Crear pods. En ese caso se creó un pods con la imagen docker nginx
   
```bash
kubectl run nginx --image=nginx
```

3. Ver el pods creado
   
```bash
kubectl get pods -o wide
```

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/4c666fc0-c1f8-405d-b4e5-b5c12f834532)

4. Revisar logs del pods 
   
```bash
kubectl logs nginx
```

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/d7db38f5-6212-450f-8deb-7340bc0e5355)
