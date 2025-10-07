# Ejercicio-Kubernetes
## Felipe Velasco

# Introducción

El proyecto Ejercicio-Kubernetes tiene como propósito reforzar el conocimiento sobre los diferentes tipos de servicios en Kubernetes y su integración con Deployments e Ingress.
A través de la creación de cuatro aplicaciones desplegadas con imágenes distintas, se busca comprender cómo se exponen los pods dentro y fuera del clúster utilizando los tipos de servicio ClusterIP, LoadBalancer, NodePort e Ingress.
Este ejercicio permite visualizar de manera práctica el flujo del tráfico hacia los contenedores, así como las diferencias y usos adecuados de cada tipo de servicio en escenarios reales de despliegue.

# Objetivo

Implementar un entorno funcional en Kubernetes compuesto por cuatro deployments independientes, cada uno asociado a un servicio diferente, con el fin de:

Identificar las características y alcances de los servicios ClusterIP, LoadBalancer, NodePort e Ingress.

Configurar la exposición de aplicaciones dentro y fuera del clúster según cada tipo de servicio.

Validar la conectividad y el acceso a los pods mediante los distintos métodos de red que ofrece Kubernetes.

Comprender el rol de los recursos Ingress para el enrutamiento de tráfico HTTP basado en nombres de host.
##  paso a paso
Crear 4 deployments, cada uno con una imagen diferente, con 4 servicios diferentes (clusterIp con nginx, loadbalancer, nodeport, ingress)

Comandos para crear los deployments:
Deployment y servicio ClusterIP:
```bash
kubectl create deployment app1 --image=nginx:alpine
kubectl expose deployment app1 --port=80 --target-port=80 --type=ClusterIP
```
Deployment y servicio LoadBalancer:
```bash
kubectl create deployment app2 --image=httpd:alpine
kubectl expose deployment app2 --port=80 --target-port=80 --type=LoadBalancer
```
Deployment y servicio NodePort:
```bash
kubectl create deployment app3 --image=redis:alpine
kubectl expose deployment app3 --port=6379 --target-port=6379 --type=NodePort
```
Deployment y servicio para Ingress:
```bash
kubectl create deployment app4 --image=nginxdemos/hello
kubectl expose deployment app4 --port=80 --target-port=80 --type=ClusterIP
```
Para el Ingress se crea un archivo llamado ingress.yaml con el siguiente contenido:

```bash
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app4-ingress
spec:
  rules:
  - host: app4.local
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: app4
            port:
              number: 80
```
y aplicarlo con:
```bash
kubectl apply -f ingress.yaml
```
![Creacion de los deployments](foto1.png)

Para Verificar:
Ver deployments:
```bash
kubectl get deployments
```
Ver pods:
```bash
kubectl get pods
```
Ver servicios:
```bash
kubectl get services
```
Ver ingress:
```bash
kubectl get ingress
```
![Verificacion de los deployments](foto2.png)

Para acceder a la página de Nginx desde fuera del clúster, se usa port-forward:
```bash
kubectl port-forward service/app1 8080:80
```
y se abre desde el localhost:8080

![Nginx](foto3.png)

# Conclusión

El ejercicio permitió consolidar los conocimientos fundamentales sobre la exposición de servicios en Kubernetes y la interacción entre los distintos componentes de red.
A través de la implementación de los cuatro tipos de servicio, se evidenció cómo ClusterIP restringe el acceso al tráfico interno del clúster, NodePort y LoadBalancer habilitan el acceso externo por diferentes mecanismos, y Ingress proporciona una capa avanzada de enrutamiento HTTP basada en nombres de dominio.
De esta manera, el proyecto demuestra la flexibilidad y potencia del modelo de red de Kubernetes, clave para el despliegue escalable y seguro de aplicaciones en entornos modernos de contenedores.

