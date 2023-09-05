# Seccion 6 y 7: Pods y Labels
Curso de capacitación

## Objetivo
El objetivo es tomar de referencia los ambientes del proyecto ficticio donde se cuenta con cuatro ambientes de los cuales “elk” es coincidente entre ellos. Cada ambiente cuenta con su nombre particular. El ambiente productivo al tener un nodo soporte se le agregan cuatro label en total.
El objetivo de las anotaciones es resaltar puntos específicos de cada ambiente

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/2167c9f6-9a38-46fc-b9bd-bda86ebadd3e)

## Procedimiento

1. Aplicamos la creación de cada pods dirijiendonos dentro del folder _manifest_

```bash
kubectl apply -f .
```

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/f6b2acec-1794-4c42-9acc-1b99fb839bf4)

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/8fb6e2e3-2818-4983-ae32-6500c9f47f4f)

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/31b1c128-71cd-4b57-9a1f-f992dcaef467)

2. ¿Qué ambiente tiene monitoreo usando elk?

```bash
kubectl get pods -l monitoreo=elk -L monitoreo
```

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/d1d14ef3-56f5-4a2e-ae74-8df67319eed8)


3. Al ambiente de desarrollor se le agregó _prometheus_ como monitoreo

```bash
kubectl label pod desarrollo monitoreo2=prometheus
```

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/f062f44e-4c26-4326-bc47-65c31731b1fa)

```bash
kubectl get pod desarrollo --show=labels
```

```bash
kubectl get pod -l monitoreo,monitoreo2 -L monitoreo -L monitoreo2
```

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/92f86bdc-3283-4c8c-8cb4-20e1e22690bb)

4. Se retiró de testing elk y se agregó datadog

```bash
kubectl get pod -l monitoreo=datadog -L monitoreo
```

```bash
kubectl get pod testing --show-labels
```

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/0bc60136-823d-415b-bdff-b248c7eae420)

5. Agregar anotaciones

```bash
kubectl get pod produccion -o=jsonpath='{.metadata.annotations"}
```

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/301eebf2-f875-4f93-a242-2729ff0a536e)

6. Se actualizó registry en el ambiente productivo
   
```bash
kubectl annotate pod producción registry="Se actualizó a registry-proy-pi"
```

![image](https://github.com/DaCriPer/k8s-Ayi-Group/assets/49571488/814c9f15-9301-43e3-80d2-88cd3aa7aed4)

