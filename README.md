# Projeto-Extra-Kubernets-
Esse projeto contém 3 análises de problemas  
Análise de problemas 1 - Encontre o problema no deploy em kubernets nome do deploy:capivara. 
Solução: 

 No terminal de comandos execute o comando:
 
 ```
 kubectl edit deployment app1-deployment -n capivara
```

Procure o nome nginx e tire o nome fake, depois dê um esc :wq salve o arquivo. 



Análise de problemas 2 - Existe um Job que está com um problema de execução, identifique o problema:

namespace: capivara
name: paco

Confirme se o namespace capivara já existe:

```
kubectl get namespaces
```

Se o namespace não existir, crie-o:

```
kubectl create namespace capivara
```

2. Crie um Arquivo YAML para o Deployment
O Deployment gerencia os Pods da aplicação. Aqui está um exemplo básico de um arquivo YAML para um Deployment que executa uma aplicação baseada em NGINX:

```
vim deployment-capivara.yaml 
```

Arquivo: deployment-capivara.yaml

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: capivara-deployment
  namespace: capivara
spec:
  replicas: 2
  selector:
    matchLabels:
      app: capivara-app
  template:
    metadata:
      labels:
        app: capivara-app
    spec:
      containers:
      - name: capivara-container
        image: nginx:latest
        ports:
        - containerPort: 80
```
        
salve o arquivo dando esc :wq 

4. (Opcional) Crie um Service para Expor a Aplicação
Se você precisar expor a aplicação, adicione um arquivo YAML para o Service. Um Service do tipo NodePort é um exemplo simples:

```
vim service-capivara.yaml
```

Arquivo: service-capivara.yaml

```
apiVersion: v1
kind: Service
metadata:
  name: capivara-service
  namespace: capivara
spec:
  selector:
    app: capivara-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort
```

 salve o arquivo e saia dando um esc :wq 

5. Aplique os Arquivos YAML
Depois de criar os arquivos YAML, aplique-os no cluster:


```
kubectl apply -f deployment-capivara.yaml
kubectl apply -f service-capivara.yaml
```

5. Verifique os Recursos Criados
Após aplicar os arquivos, verifique os recursos no namespace capivara:

```
kubectl get all -n capivara
```

6. Acesse a Aplicação

Se você criou um Service do tipo NodePort, pode acessar a aplicação pelo IP de um nó do cluster e a porta exposta. Para descobrir a porta, use:

```
kubectl get service capivara-service -n capivara
```

Acesse a aplicação no navegador ou com curl:

```
curl http://<IP-do-nó>:<NodePort
```

Análise de problemas 3 - Crie um pod com o nome teste-pod com a imagem: nginx no namespace default 

Solução: 

Execute o comando vim teste-pod.yaml para criar o arquivo abaixo:

Arquivo: teste-pod.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: teste-pod
  namespace: default
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
    ports:
    - containerPort: 80
```


Passo para Criar o Pod
Salve o conteúdo acima em um arquivo chamado teste-pod.yaml.
Execute o comando abaixo para criar o Pod:

```
kubectl apply -f teste-pod.yaml
```

2. Verifique se o Pod Foi Criado
Após criar o Pod, verifique o status:

```
kubectl get pods -n default
```

Se precisar inspecionar o Pod:

```
kubectl describe pod teste-pod -n default
```

Para verificar os logs do Pod:

```
kubectl logs teste-pod -n default
```

Se preferir, você pode criar o Pod diretamente sem um arquivo YAML usando o comando kubectl run:

```
kubectl run teste-pod --image=nginx --restart=Never --port=80 -n default
```
