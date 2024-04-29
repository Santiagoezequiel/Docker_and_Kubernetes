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

<font color="grey>Este comando se utiliza para crear o actualizar recursos en un cluster a partir de archivos de configuracion YAML o JSON</font>

