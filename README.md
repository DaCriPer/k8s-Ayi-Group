# Proyecto integrador
Curso de capacitación

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/48293220-6a69-470f-bd73-989a728c9483)


## Requerimientos

Debe estar basado en drupal como interfaz web y contar con mariadb como base de datos
Debe contar con el dominio pi-k8s.ayi.group como dominio del ingress
Los servicios definidos para drupal y mariadb no deben ser consumidos exteriormente por su puerto. Debe ser el ingress controller quien acceda a ellos.
Debe contar con una división lógica de nombre drupal-mariadb
El proyecto debe ser capaz de ser reactivo cuando se eliminen los pods levantando correctamente las nuevas replicas.
Al momento de navegar por la web de drupal, los datos almacenados deben aparecer sin importar cuál réplica de drupal haya recibido las peticiones.

## Requisitos previos
Debe tener un controlador de Ingress para satisfacer un Ingress. Solo crear un recurso de Ingress no tiene efecto.
Ejecute _minikube start_ para crear un clúster.

### Habilitar el controlador de ingreso

Para habilitar el controlador de entrada NGINX, ejecute el siguiente comando

```bash
minikube addons enable ingress
```

Verifique que el controlador de entrada NGINX se esté ejecutando

```bash
kubectl get pods -n ingress-nginx
```
Nota: Puede pasar hasta un minuto antes de que vea que estos pods funcionan correctamente.

La salida es similar a:
NAME                                        READY   STATUS      RESTARTS    AGE
ingress-nginx-admission-create-g9g49        0/1     Completed   0          11m
ingress-nginx-admission-patch-rqp78         0/1     Completed   1          11m
ingress-nginx-controller-59b45fb494-26npt   1/1     Running     0          11m

Para habilitar el controlador  dns de entrada NGINX, ejecute el siguiente comando:

```bash
minikube addons enable ingress-dns
```

### Actualizar el archivo host 

#### Obtener la ip de minikube

```bash
minikube ip
```

#### Mapear el host de ingress con la ip de minikube

- Para sistemas linux, dirijirse a _/etc/hosts_
- Para sistemas window, dirijirse a _C:\Windows\System32\drivers\etc\hosts_

```
<ip_minikube> pi-k8s.ayi.group
```

## Namespace

```bash
kubctl create namespace proyecto-integrador
```

## Ingress controller

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/f5137edf-ac7c-45fe-849c-a4c469021192)

Se define el ingress controller para el dominio  pi-k8s.ayi.group redireccionando al servicios de drupal.
Ingress expone las rutas HTTP y HTTPS desde fuera del clúster a los servicios dentro del clúster. El enrutamiento del tráfico está controlado por reglas definidas en el recurso Ingress.
Se puede configurar un Ingress para brindar servicios de direcciones URL accesibles externamente, equilibrar la carga del tráfico, finalizar SSL/TLS y ofrecer alojamiento virtual basado en el nombre. Un controlador de Ingress es responsable de cumplir con el Ingress, generalmente con un balanceador de carga, aunque también puede configurar su enrutador perimetral o interfaces adicionales para ayudar a manejar el tráfico.
Un Ingress no expone puertos o protocolos arbitrarios. La exposición de servicios distintos de HTTP y HTTPS a Internet suele utilizar un servicio de tipo Service.Type=NodePort o Service.Type=LoadBalancer .

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/22958526-8992-4e61-a391-f4d96436cf02)

## Drupal

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/69f3d19e-147f-4900-8ef1-dfb9078d95e1)

## volume persistente

```bash
kubectl apply -f pv.yaml
```

```bash
devuser@debian11-2:~/proyectoKube$ kubectl get pv -n proyecto-integrador
NAME           CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM                                 STORAGECLASS   REASON   AGE
volume-local   1Gi        RWO            Retain           Available   proyecto-integrador/storage-local-1   volume.local            18s
devuser@debian11-2:~/proyectoKube$ 
```

## reclamado de volumen persistente

```bash
kubectl apply -f pvc.yaml
```

```bash
devuser@debian11-2:~/proyectoKube$ kubectl get pv,pvc -n proyecto-integrador
NAME                            CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                                 STORAGECLASS   REASON   AGE
persistentvolume/volume-local   1Gi        RWO            Retain           Bound    proyecto-integrador/storage-local-1   volume.local            7m16s

NAME                                    STATUS   VOLUME         CAPACITY   ACCESS MODES   STORAGECLASS   AGE
persistentvolumeclaim/storage-local-1   Bound    volume-local   1Gi        RWO            volume.local   6s
devuser@debian11-2:~/proyectoKube$ 
```

## Maria db

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/5ee0b117-29d0-479a-9f71-585e6c4796cb)

```bash
devuser@debian11-2:~/proyectoKube$ kubectl get all -n proyecto-integrador
NAME                          READY   STATUS    RESTARTS   AGE
pod/drupal-56d68d8c9d-2mtrk   1/1     Running   0          14m
pod/drupal-56d68d8c9d-phjkr   1/1     Running   0          14m
pod/mariadb-vol               1/1     Running   0          14m

NAME                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/drupal-service   NodePort    10.102.255.171   <none>        80:32674/TCP   14m
service/mariadb-vol      ClusterIP   10.104.246.35    <none>        3306/TCP       14m

NAME                     READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/drupal   2/2     2            2           14m

NAME                                DESIRED   CURRENT   READY   AGE
replicaset.apps/drupal-56d68d8c9d   2         2         2       14m
devuser@debian11-2:~/proyectoKube$
```
## Inconvenientes no resueltos

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/a971ddc8-5394-42e6-a8e0-137aedafecc0)

Al momento de configurar drupal en la web con mariadb, se obtiene error de parte de mariadb.
Los logs de mariadb mencionan
```
borted connection 5 to db: 'unconnected' user: 'unauthenticated' host: '10.244.0.234
```
