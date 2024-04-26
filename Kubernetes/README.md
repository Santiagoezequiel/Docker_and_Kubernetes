# KUBERNETES

Es un sistema de codigo abierto de orquestacion de contenedores.

Permite el despliegue, el escalamiento y la administracion automatica de aplicaciones.

### *¿Por que utilizarlo?*

- Service Discovery
- Load balancing
- Self-Healing
- Horizontal Scaling
- Secret/Setting
- Batch porcessing
- Storage orchestration

### Consideraciones
- Usa contenedores (Docker y otros)
- Todo son objetos
- Todo se define con manifiestos (YAML)
- Si KUBERNETES muere la aplicacion puede seguir funcionando
- Los proveedores cloud tienen servicios de administracion de Kubernetes


### Configuracion imperativa

Tu defines los estados deseados de los objetos que deseas y el se encarga de mantenerlos asi.

### ¿Como nos comunicamos con K8s?

- Herramientas de terceros (DASHBOARD, TERRAFORM)
- HTTP/API RESET
- CLI

### ¿Donde podemos desplegar K8s?

- Local
- En maquinas virtuales en la nube
- Usar los K8s como servicios de los proveedores cloud
    - EKS AmazonCloud
    - GKS GoogleCloud
    - AKS AzureCloud




## ESTRUCTURA DE KUBERNETES

    USER    (Local Environment)
    |
    |_ _ MASTER (Master Node)
            |
            |_ _ _ _ WORKER (Node)
            |_ _ _ _ WORKER (Node)

### Local Environment
Es el entorno de desarrollo o prueba donde se configura y ejecuta Kubernetes localmente, a menudo mediante herramientas como MiniKube o Docker Desktop Kubernetes. En este entorno se pueden ejecutar **Master Nodes** y **Nodes** de trabajo en una unica maquina para simular un clúster completo de Kubernetes.

### Master Nodes
Es el componente principal del clúster de Kubernetes. El nodo maestro controla y coordina las operaciones del clúster. Está compuesto por varios componentes, como el API Server, el Control Manager, el Scheduler y el Etcd. El **etcd** es un almacen de datos distribuido que almacena el dato del clúster.

- **Api server**
    -   Es el componente central del master y del clúster de K8s en general.
    -   Expone la API de K8s, que permite a los usuarios y a otros componentes del clúster comunicarse y gestionar
        recursos
    -   Todas las operaciones de administracion y gestion, como la creacion, modificacion y eliminacion de recursos (como
        pods, servicios, volumenes, etc.), se realizan a traves de Api Server.
    -   El ApiServer valida y autentica las solicitudes, y luego las procesa, almacenando el estado del clúster en etcd.

 - **Control Manager**
    -   Es un conjunto de controladores que son responsables de observar el estado del clúster a traves del API Server y
        tomar acciones para mantener el estado deseado.
    -   Cada controlador se enfoca en un aspecto particular del cluster, como la replicacion, los recursos de
        almacenamiento, los servidores de red, etc.
    -   Por ejemplo, el controlador de replicacion garantiza que el numero deseado de replicas de un pod este siempre en
        funcionamiento, mientras que el controlador de servicios asegura que los servicios esten disponibles y enrutados correctamente.


- **Scheduler**
    -   Es responsable de asignar pods recien creados a nodos disponibles en el clúster.
    -   Examinar los requisitos de recursos y las restricciones de afinidad y anti-afinidad
        de los pods para tomar decisiones sobre donde programarlos
    -   El Scheduler considera factores como la capacidad de recursos de los nodos, las restriciones de afinidad y
        anti-afinidad, y las politicas de calidad de servicios al programar los pods.

- **Etcd**
    -   Es un almacenamiento de datos distribuido y consistente que se utiliza para almacenar el estado del clúster de K8s
    -   Todos los componentes del nodo master (ApiServer,Control Manager,Scheduler) leen y escriben en etcd para mantener
        y consultar el estado del clúster.
    -   Proporciona un mecanismo confiable para almacenar y distribuir informacion de configuracion y estado critico del 
        clúster, lo que garantiza la coherencia y la disponibilidad incluso  en situaciones de fallo del nodo master.




### Nodes
Son las maquinas fisicas o virtuales donde se ejecutan los contenedores. Cada nodo ejecuta los servicios necesarios para ejecutar pods, que son las unidades mas pequeñas y desplegables de trabajo en Kubernetes.
Cada nodo tiene una capacidad limitada de recursos como CPU, memoria y almacenamiento.Y cada uno de estos son desechables.

### Estructura de un YAML
YAML es un formato de serializacion de datos legibles por humanos inspirado en lenguajes como XML

Todos los objetos a partir de aqui tienen un yml para configurarlos.


### **Cluster**
Un cluster es el conjunto de nodos que ejecutan el software de Kubernetes y estan conectados entre si. Este cluster incluye nodos de diferentes roles: **Master nodes** y **Worker Nodes**.


## MANIFIESTO Y OBJETOS

- Namespace
- Pod
- Deployment
- Servicio
- Ingress
- Replicaset/ReplicationController
- HPA
- VPA
- Persistent Volumen
- Storage


### Orden de los objetos

    Service
      |_________ Deployment
                    |___________ Pod

**Pod**
Un pod es la unidad mas pequeña y basica en Kubernetes. Representa un entorno de ejecucion para unao mas instancias de un contenedor. Los contenedores dentro de un pod comparten recursos como la red y el almacenamiento, y se ejecutan en la misma maquina virtual o fisica. Los pods son efimeros y pueden ser creados, escalados y destruidos de manera dinamica por Kubernetes segun sea necesario.

Los pods no son inmortales.
Un deployment hace inmortal a un pod
Un pod tiene un ip, per los ip de los pods no son fijos.

**Deployment**
Un Deployment es un objeto en kubernetes que maneja la creacion y escalado de replicas de pods. Permite definir el estado deseado de las aplicaciones y asegura que ese estado se mantenga incluso incluso si los pods fallan o se eliminan. Los deployments permiten realizar actualizaciones de manera controlada, facilitando el despliegue de nuevas versiones de aplicaciones sin tiempo de inactividad.

**Service**
Un service es un objeto que define un conjunto logico de Pods y una politica por la cual acceder a ellos. Proporciona una forma persistente de acceder a una aplicacion, independientemente de la ubicacion o el estado de los pods individuales. Los service pueden ser de diferentes tipos, como **ClusterIP**(Acceso interno solo dentro del clúster), **NodePort**(Acceso externoa traves de un puerto en cada nodo), **LoadBalancer**(Balanceo de carga externo a traves de un equilibrador de carga de red), o ExternalName(Mapeo a un registro DNS externo)

Los servicios comunican a los pods con el exterior a traves del kube-proxy.
Un servicios puede definir mas de u puerto
El servicio usa el protocolo TCP
Se puede cambiar de protocolo
Un servicio puede apuntar a otros servicio
Un servicio puede apuntar a otra cosa que no sea un pod




                                        Aplicacion Frontend
                                               |
                                            Servicio
                          _____________________|________________________
                          |                    |                       | 
                        Worker               Worker                  Worker
                      kube-proxy           kube-proxy              kube-proxy

      Worker
        |
        |
    Deployment
        |
       Pods


### Tipos de Servicios

**ClusterIP** expone el servicio internamente.
**NodePort** expone el servicio al exterior en la ip de cada nodo y usa un puerto estatico.
**LoadBalancer** expone un servicio externamente utilizando un balanceador de carga de un proveedor de nube.
**ExternaName** asigna el servicio al contenido del externalName devolciendo CNAME registro.




### Ingress
Ingress redirije las solicitudes de los usuarios a los diferentes servidores segun las reglas definidas por el usuario.
En lugar de exponer cada servicio individualmente al trafico externo, lo que es dificil de manejar, ingress proporciona una forma de consolidar y gestionar la exposicion de multiples servicios bajo un solo punto de entrada. Esto es util en entornos de produccion donde se necesitan multiples servicios accesibles publicamente.

**Caracteristicas**
-   Balanceo de cargas: Permite distribuir el trafico entre varios pods de un servicio para mejorar el rendimiento.
-   TSL/SSL: Soporta la terminacion SSL para servicios HTTPS, proporcionando una capa adicional de seeguridad. 

**Estructura**

    Exterior
        |_______Ingress
                    |______Service
                              |_________ Deployment
                                             |___________ Pod



### Namespace
Son espacios de nombres, que limitan lo que puedes ver, cuando listas los objetos que estan en un namespace solo puede ver los que previamente se configuraron en el.
Normalmente existen NS preconfigurados en los kubernetes.
- Default
- Kube-public
- Kube-system






## HPA Y VPA

En resumen, el VPA se encarga de ajustar verticalmente los recursos asignados a los pods individuales, mientras que el HPA se encarga de ajustar horizontalmente el número de réplicas de conjuntos de pods para manejar la carga de trabajo. Ambos son componentes importantes para lograr una escalabilidad eficiente y automática en Kubernetes.

### Verticalo Pods Autoscaler (VPA)
Supongamos que inicialmente asignaste a cada pod 1 vCPU y 1 GB de memoria, pero a medida que la aplicación comienza a recibir más tráfico, algunos pods podrían requerir más recursos para manejar la carga.
El VPA monitorea el uso de CPU y memoria de cada pod individual y nota que algunos están alcanzando su límite.
Entonces, el VPA ajusta automáticamente los límites de recursos de esos pods específicos, aumentando la asignación de CPU y memoria a 2 vCPU y 2 GB de memoria, respectivamente.
Esto ayuda a garantizar que cada pod tenga suficientes recursos para manejar la carga sin sobrecargarse ni ralentizar la aplicación.


### Horizontal Pods Autoscaler (HPA)
Supongamos que, además de los cambios verticales en los recursos de los pods, la carga de trabajo de la aplicación también varía durante el día.
Durante las horas pico, la aplicación experimenta un aumento significativo en el tráfico, lo que podría abrumar a los pods existentes si no se escalan.
El HPA monitorea la utilización de recursos de los pods y nota que están operando al límite de su capacidad.
Entonces, el HPA automáticamente escala horizontalmente el número de réplicas de la aplicación, creando más pods para manejar el aumento de tráfico.
Cuando la carga de trabajo disminuye nuevamente, el HPA reduce el número de réplicas para evitar el gasto innecesario de recursos.






### HPA
**¿Como se escalá con HPA?**

Primero debes:
- Definir el objeto que deseas escalar
- Definir el minimo de replicas
- Definir el maximo de replicas
- Definir el recurso a monitorear
- Definir la regla para escalar.


### VPA
**¿Como se escalá con VPA?**

Consideraciones:
- No agrega nuevos pods, reemplaza el pod existente.
- El VPA cuando necesita escalar, destruye el pod existente y crea uno nuevo
- Es automatico el escalado.
- El escalado puede ser para arriba y para abajo.
- Puedes definir que no será analizado dentro del pod (que container no se tomará en cuenta para el escalado)


**Sugerencias de VPA**

Las Recomendaciones VPA (Vertical Pod Autoscaler Recommendations) son sugerencias proporcionadas por el Vertical Pod Autoscaler (VPA) en Kubernetes sobre cómo deberían ajustarse los recursos asignados a los pods para optimizar el rendimiento y la eficiencia. Estas recomendaciones se basan en el análisis del comportamiento pasado de los pods y su uso de recursos.

El VPA recopila datos sobre el uso de CPU y memoria de los pods y utiliza algoritmos para predecir cuántos recursos necesitarán esos pods en el futuro. Basándose en esta información, el VPA puede sugerir aumentar o disminuir los límites de recursos asignados a los pods.

Las recomendaciones VPA pueden incluir sugerencias para aumentar o disminuir los límites de CPU y memoria de un pod, así como también pueden incluir recomendaciones sobre otros recursos, como la asignación de almacenamiento.

Estas recomendaciones pueden ser útiles para los administradores de clústeres y desarrolladores para garantizar que los pods tengan suficientes recursos para manejar la carga de trabajo de manera eficiente sin desperdiciar recursos innecesariamente. Sin embargo, es importante tener en cuenta que estas recomendaciones son solo sugerencias y pueden necesitar ser evaluadas y ajustadas según las necesidades y características específicas de cada aplicación y entorno.





## Storage

El almacenamiento en Kubernetes es fundamental para muchas aplicaciones, especialmente aquellas que necesitan mantener datos persistentes, como bases de datos, sistemas de archivos compartidos, repositorios de archivos, etc. Los administradores de clústeres y los desarrolladores deben elegir y configurar cuidadosamente el almacenamiento adecuado para satisfacer las necesidades de sus aplicaciones y garantizar la integridad y disponibilidad de los datos.

**COMPORTAMINETO POR DEFECTO**

- Los pod no guardan informacion
- El disco de los pods es efimero
- La info se pierde cuando el pod muere
- No comparte info entre pod del mismo tipo
- Se pueden definir tipos de volumenes, para cambiar el comportamiento por defecto
- La forma en que se crea y el ciclo de vida de la data depende del tipo de volumen creado

- POR DEFECTO los contenedores no comparten espacio en disco aunque esten en el mismo pod.

**Ilustracions**

    Node                                            Node
     |                                               |
     |------- Pod ----- Storage                      |------- Pod ----- Storage
     |                                               |
     |------- Pod ----- Storage                      |------- Pod ----- Storage   
     |                                               |
     |------- Pod ----- Storage                      |------- Pod ----- Storage 








## Visto hasta el momento..

Hasta el momento esta es la estructura que he aprendido de un Clúster en K8S

    Clúster
       |
       |____ Exterior
                 |_______ Ingress
                             |______ Service
                                       |_________ Deployment
                                                      |___________ Pod
                                                                      |__________ Container
                                                                                    |_________ Storage


**Clúster:** Representa el entorno de Kubernetes en el que se ejecutan todos los recursos.
**Exterior:** Representa el entorno externo al clúster, como Internet.
**Ingress:** Administra el acceso externo a los servicios dentro del clúster.
**Service:** Expone la aplicación web como un servicio de red dentro del clúster.
**Deployment:** Administra un conjunto de réplicas de nuestros pods que ejecutan la aplicación web.
**Pod:** Es la unidad básica de implementación en Kubernetes, cada uno puede contener uno o más contenedores que ejecutan
         la aplicación.
**Container:** Dentro de cada pod, hay uno o más contenedores que ejecutan los componentes de la aplicación.
**Storage:** Proporciona almacenamiento persistente asociado a nuestra aplicación.
