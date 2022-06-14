# Informações ref. ao KUBERNETES

## Comandos:

### Listar pods:
    kubectl get pods

### Deletar um pod:
    kubectl delete pod <nome do pod>

### Replicasets:
Define um número de réplicas de pods.
Caso um pod pare de funcionar, outro assume no lugar.
Sempre mantém o número de réplicas no ar conforme o definido.

### Listar os replicasets:
    kubectl get rs

### Criando um deployment:
    kubectl apply -f .\nginx-deployment.yaml

### Listar os deployments:
    kubectl get deployments

### Verificar histórico/versões dos deployments:
    kubectl rollout history deployment nginx-deployment

### Seta o comando na anotação (histórico):
    kubectl apply -f .\nginx-deployment.yaml --record

### Altera a anotação do histórico:
    kubectl annotate deployment nginx-deployment kubernetes.io/change-cause="Definindo a imagem com versão latest"

### Fazendo rollback de versão:
    kubectl rollout undo deployment nginx-deployment --to-revision=1

### Excluindo um deployment:
    kubectl delete deployment nginx-deployment

### Volumes:
Volumes são independentes dos containers do pod porém são dependentes do pod.

Exemplo de pod-volume.yaml.

    apiVersion: apps/v1
    kind: Pod
    metadata:
    name: pod-volume
    spec:
        containers:
          - name: nginx-container
            image: nginx:latest
            volumeMounts:
                - mountPath: /volume-dentro-do-container
                  name: segundo-volume
             - name: jenkins
               image: jenkins/jenkins:alpine
            volumeMounts:
                - mountPath: /volume-dentro-do-container
                  name: segundo-volume
      volumes:
       - name: segundo-volume
         hostPath:
           path: /home/segundo-volume
                 type: DirectoryOrCreate

### Persistência com PersistentVolumes:
Criando um PersistentVolume com um disco do Google Cloud Platform.
Necessário criar um disco no GCP.
Executar a criação de um PersistentVolume no cloudshell do GCP conforme código abaixo:

kubectl apply –f pv.yaml

    apiVersion: v1
    kind: PersistVolume
    metadata:
        name: pv-1
    spec:
        capacity:
            storage: 10Gi
        accessModes:
            - ReadWriteOnce
        gcePersistentDisk:
            pdName: pv-disk
        storageClassName: standard

Executar a criação de um PersistentVolumeClaim no cloudshell do GCP conforme código abaixo:
PersistentVolumeClaim são utilizados para acessar o volume.

kubectl apply –f pvc.yaml

    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
        name: pvc-1
    spec:
        accessModes:
            - ReadWriteOnde
        resources:
            requests:
                storage: 10Gi
        storageClassName: standard

Executar a criação de um Pod que acessa o PersistentVolume no cloudshell do GCP conforme código abaixo:

kubectl apply –f pod-pc.yaml

    apiVersion: v1
    kind: Pod
    metadata:
        name: pod-pv
    spec:
        containers:
            - name: nginx-container
                image: nginx-latest
                volumeMounts:
                    - mountPath: /volume-dentro-do-container
                    name: primeiro-pv
        volumes:
            - name: primeiro-pv
                hostPath:
                    persistentVolumeClaim:
                        claimName: pvc-1

[Link para documentação oficial de PersistentVolumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)

### Storage Classes:
Storage Classes são criadas para gerenciar discos dinamicamente.

Provisionar um disco no Google Cloud Platform, criando o Storage Class:

kubectl apply –f pod-pc.yaml

    apiVersion: storage.k8s.io/v1
    kind: StorageClass
    metadata:
        name: slow
    provisioner: kubernetes.io/gce-pd
    parameters:
        type: pd-standard
        fstype: ext4
        replication-type: none

Criar discos dinamicamente e PV dinamicamente:

kubectl apply –f pvc-sc.yaml

    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
        name: pvc-2
    spec:
        accessModes:
            - ReadWriteOnde
        resources:
            requests:
                storage: 10Gi
        storageClassName: slow

kubectl apply –f pod-sc.yaml

    apiVersion: v1
    kind: Pod
    metadata:
      name: pod-sc
    spec:
      containers:
        - name: nginx-container
          image: nginx:latest
          volumeMounts:
            - mountPath: /volume-dentro-do-container
              name: primeiro-pv
      volumes:
        - name: primeiro-pv
          persistentVolumeClaim:
            claimName: pvc-2


### StatefulSets:

StatefulSets são utilizados para que em caso de falha de um POD, os dados sejam persistidos e recuperados em uma recriação automática do POD.

Criando um PVC para imagens:

kubectl apply –f imagens-pvc.yaml

    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: imagens-pvc
    spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: 1Gi


Criando um PVC para a sessão:

kubectl apply –f sessao-pvc.yaml

    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: sessao-pvc
    spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: 1Gi


Criando o statefulset:

kubectl apply –f sistema-noticias-statefulset.yaml

    apiVersion: apps/v1
    kind: StatefulSet
    metadata:
      name: sistema-noticias-statefulset
    spec:
      replicas: 1
      template:
        metadata:
          labels:
            app: sistema-noticias
          name: sistema-noticias
        spec:
          containers:
            - name: sistema-noticias-container
              image: aluracursos/sistema-noticias:1
              ports:
                - containerPort: 80
              envFrom:
                - configMapRef:
                    name: sistema-configmap
              volumeMounts:
                - name: imagens
                  mountPath: /var/www/html/uploads
                - name: sessao
                  mountPath: /tmp
          volumes:
            - name: imagens
              persistentVolumeClaim:
                claimName: imagens-pvc
            - name: sessao
              persistentVolumeClaim:
                claimName: sessao-pvc
      selector:
        matchLabels:
          app: sistema-noticias
      serviceName: svc-sistema-noticias


### Liveness Probes (Prova de vida):
Torna visível ao Kubernetes que uma aplicação não está se comportando da maneira esperada.
Indicará falha caso o código de retorno seja menor que 200 ou maior/igual a 400.
Exemplo: Caso a aplicação dentro de um POD responda um erro 500, testar 3 vezes e caso persista o erro, reiniciar o POD.

Exemplos constam no arquivo kubernetes-parte2-Aula4.zip.

### Readiness Probes:
Exemplo: Se um POD está inicializando, realizar um teste de GET, se aplicação estiver OK, passa a enviar requisições para este POD.

Exemplos constam no arquivo kubernetes-parte2-Aula4.zip.

### Startup Probe:
Há um terceiro tipo de probe voltado para aplicações legadas, o Startup Probe. Algumas aplicações legadas exigem tempo adicional para inicializar na primeira vez. Nem sempre Liveness ou Readiness Probes vão conseguir resolver de maneira simples os problemas de inicialização de aplicações legadas. Mais informações sobre Startup Probes podem ser adquiridas por [aqui](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-startup-probes).

### Escalando pods automaticamente:
Caso falte recursos em um POD, pode-se utilizar o HorizontalPodAutoscaler para definir o escalonamento automático de pods.
Exemplo: Caso falte CPU, crie um pod novo. Caso a utilização de memória passe de um limite, crie um pod novo.

### Utilizando o HPA no Windows:
[Link do projeto no GitHub do metrics-server.](https://github.com/kubernetes-sigs/metrics-server/)
[Para baixar o arquivo de definição do servidor de métricas, clique aqui.](https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.3.7/components.yaml)


Para desabilitar uma verificação de certificados:
Podemos desabilitar com essa flag --kubelet-insecure-tls
No caso do Windows, nós vamos precisar definir essa flag dentro do nosso arquivo de definição.
Então, um traço aqui em args e definimos os --kubelet-insecure-tls


Para criar o servidor de métricas:

    kubectl apply –f components.yaml


Para listar o hpa:

    kubectl get hpa


Para assistir como está a situação do hpa:
    kubectl get hpa --watch


[Para fazer o download da ferramenta Git Bash, basta acessar esse link.](https://git-scm.com/download/win)

### Exemplo de arquivo declarativo de HPA:

Neste exemplo, o HPA visa manter o consumo médio de CPU o mais próximo de 20%.

    apiVersion: autoscaling/v2beta2
    kind: HorizontalPodAutoscaler
    metadata:
      name: primeiro-hpa
    spec:
      scaleTargetRef:
        apiVersion: apps/v1
        kind: Deployment
        name: primeiro-deployment
      minReplicas: 1
      maxReplicas: 5
      metrics:
        - type: Resource
          resource:
            name: cpu
            target:
              type: Utilization
              averageUtilization: 20

### Utilizando o HPA no Linux:

Para utilizar o servidor de métricas no Linux, utilizaremos o Minikube.

Para habilitar no linux:

    minikube addons enable metrics-server


Algumas configurações extras são necessárias para utilizar o VerticalPodAutoscaler.
Mais informações podem ser obtidas nesse [link](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler).

Também podemos ver mais informações específicas sobre diferentes Cloud Providers:
[GCP](https://cloud.google.com/kubernetes-engine/docs/concepts/verticalpodautoscaler)
[AWS](https://docs.aws.amazon.com/eks/latest/userguide/vertical-pod-autoscaler.html)



## Comandos para Azure:

Para acessar um container no Azure.

    kubectl exec -it <pod_name> --namespace <namespace_name> -c <container_name> -- /bin/bash