# 🚀 Infraestructura como Código (IaC) y Escalado Horizontal en Kubernet

Este proyecto demuestra la implementación de una arquitectura resiliente y elástica para un servicio crítico de banca (**pagos-service**). Utilizando manifiestos declarativos de Kubernetes, se simula la gestión de un pico de tráfico masivo mediante escalado horizontal (HPA).

Lo primero es bajar los manifiestos a su entorno local o instancia de nube (como Killercoda):

git clone https://github.com/dbraca11/k8s-iac-scaling.git
cd k8s-iac-scaling

Paso 2: Despliegue de la Infraestructura (IaC)
Gracias al enfoque declarativo de Kubernetes, desplegamos el servicio de pagos con 3 réplicas iniciales para garantizar alta disponibilidad:

Bash
kubectl apply -f pagos-app.yaml

Paso 3: Verificación de Disponibilidad
Comprobamos que los pods estén correctamente orquestados y en estado Running:
kubectl get pods

Paso 4: Simulación de Escalado de Emergencia (Lo que verán en pantalla)
Ante un escenario de alta demanda, ejecutamos un escalado horizontal para subir de 3 a 8 réplicas instantáneamente:
sed -i 's/replicas: 3/replicas: 8/' pagos-app.yaml && kubectl apply -f pagos-app.yaml

Qué sucede durante la emulación?

Crecimiento en Vivo: Al ejecutar kubectl get pods -w, verán cómo Kubernetes crea automáticamente 5 nuevas instancias en segundos.

Resiliencia: El tráfico se distribuye entre los nuevos nodos sin interrumpir el servicio.

Auto-gestión: Kubernetes se encarga de que el estado actual siempre coincida con el estado deseado en el archivo YAML.

Paso 5: Verificación de Salud (Observabilidad)
Para demostrar que las nuevas instancias están listas para procesar pagos, revisamos los logs internos de uno de los nuevos pods:

kubectl logs [nombre-del-pod]

Paso 6: Optimización de Recursos (Scale Down)
Una vez superado el pico de tráfico, procedemos a liberar recursos para optimizar costos de nube, volviendo a las 3 réplicas originales:

sed -i 's/replicas: 8/replicas: 3/' pagos-app.yaml && kubectl apply -f pagos-app.yaml
