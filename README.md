# Seccion 8, 9 y 10: Instalación sin volumes
Curso de capacitación

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/9a9e960b-a55f-4d19-a61d-755eb8d44e75)

1. Creamos el deploy y service de drupal

```bash
kubectl apply -f drupal_without_vol.yaml
```

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/521e3a5b-97e9-4489-84b6-92d3b3698234)

2. Creamos el deploy y servicio de mariadb

```bash
kubectl apply -f drupal_without_vol.yaml
```

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/a650226e-c40e-40ff-9841-32bf93187d5b)

3. Obtenemos la ip de minikube, el puerto del servicio NodePort  y consultamos en la web

```bash
minikube ip
kubectl get svc | grep drupal
```

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/9b1f45d8-adfc-4bf4-bd1f-aeafc5ed2e64)


4. configuramos la base en dato en drupal

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/f0a37542-b498-434b-9061-e44cc955474f)

5. Una vez finalizada la configuración de drupal, poner a prueba el concepto de “volumen efímero” donde al eliminar los pods, se elimina toda configuración que se realizó perdiendo todos los avances. De ahí la importancia de los volúmenes persistentes

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/a92e7023-bfa4-4909-9676-7ff71deb747c)

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/dfc7245d-adfc-447f-8d8c-cd2542b5830d)
 
