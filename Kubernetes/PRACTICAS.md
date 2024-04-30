# Ejercisios practicos en K8S

### 1. Crea un pod llamado **my-pod** de la imagen **nging:alpine**

    k run my-pod --image=nginx:alpine

### 2. Elimina el pod creado en el punto 1.

    k delete pod my-pod

### 3. Crea un deployment llamado my-deployment de la imagen nginx:alpine.

    k create deploymente my-deployment --image=nginx:alpine

### 4. Escala el deployment creado para que ejecute 3 replicas, luego verifica que esten listas.

    k scale deployment/my-deployment --replicas=3

    k get deployment my-deployment

### 5. Reduce las replicas del deployment a 2, luego verifica que esten listas.

    k scale deployment/my-deployment --replicas=2

    k get deployment my-deployment

### 6. Cambia la imagen del deployment de **nginx:alpine** a **httpd:alpine**.

    k set image deployment my-deployment nginx=httpd:alpine

### 7. Elima el deployment ya creado.

    k delete deployment my-deployment

### 8. Instala el panel YAML de K8s personalizado.

    kubectl apply -f /root/dashboard.yaml

Este comando se utiliza para crear o actualizar recursos en un cluster a partir de archivos de configuracion YAML o JSON

    kubectl -n kubernetes-dashboard wait --for=condition=ready pod --all

Este comando espera a que ciertas condiciones se cumplan en el recurso. Aqui, esta esperando a que todos los pods en el namespace **kubernetes-dashboard** esten listos y en un estado **ready**.


**Las modificaciones aqui fueron estos argumentos:**

    args:
    - --namespace=kubernetes-dashboard
    - --enable-skip-login
    - --disablee-settings-authorizer
    - --enable-insecure-login
    - --insecure-bind-address=0.0.0.0

**Y un servicio actualizado YAML:**

    kind: Service
    apiVersion: v1
    metadata:
        labels:
            k8s-app: kubernetes-dashboard
        name: kubernetes-dashboard
        namespace: kubernetes-dashboard
    spec:
        ports:
            - port: 9090
              targetPort: 9090
        selector:
            k8s-app: kubernetes-dashboard


### 9. Crea una cuenta de servicio y use el token.

    kubectl -n kubernetes-dashboard create sa admin-user

Este comando crea un nuevo **Service Account** llamado **admin-user** en el namespace **kubernetes-dashboard**. Los Service Accounts son identidades que se utilizan para autenticar y autorizar las interacciones de los pods con otros componentes del clúster de Kubernetes.

    kubectl create clusterrolebinding admin-user --clusterrole cluster-admin --serviceaccount kubernetes-dashboard:admin-user

Este comando crea un ClusterRoleBinding (asociacion de roles a nivel de cluster) llamado **admin-user**. Asocia el ClusterRole **cluster-admin** al Service Account  **admin-user** en el namespace **kubernetes-dashboard**. El ClusterRole **cluster-admin** es un rol predefinido en Kubernetes que otorga permisos de administrador sobre todo el cluster.

    kubectl -n kubernetes-dashboard create token admin-user

Este comando crea un token de acceso para el Service Account **admin-user** en el namespace **kubernetes-dashboard**. Este token de acceso se utiliza para  autenticar al usuario administrador cuando accede al dashboard de Kubernetes.