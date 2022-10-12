# K3S

K3S indicado para arquitetura Edge , IOT e Pipeline CI/CD.


## Máquinas Virtuais
Criar 01 máquina virtual que será o Server e irá fazer o papel do Control-Pane, esta máquina deve ter no minimo 2GB e 1 CPU.

As outras 02 máquinas virturias serãos os Agents, irão fazer o papel de WorkNodes, o minimo exigido é de 1GB e 1 CPU.

O Cloud Provider que utlizado para este laboratório foi a Digital Ocean.

Foram criados 03 Droplets.

Identificação do Server:
k8s-server-1

Identificação do Agents:
k8s-agent-2
k8s-agent-3


## Instalar o K3S

Instalar K3s na máquina k8s-server-1:
``` bash
#acessar a máquina virtual
ssh root@ipDoServer

# Instalar o k3s
curl -sfL https://get.k3s.io | sh -

# Checar os nodes
Kubectl get nodes
```

Na máquina k8s-server-1 anotar o Token:
``` bash
cat /var/lib/rancher/k3s/server/node-token
```

## Join do Agent com Server:


Instalar o K3S e realizar o JOIN ma máquina k8s-agent-2:

``` bash
#acessar a máquina virtual k8s-agent-2
ssh root@ipDoAgentK8sAgent2

#executar o comando
curl -sfL https://get.k3s.io | K3S_URL=https://ipDoServer:6443 K3S_TOKEN=TOKEN sh -
```

curl -sfL https://get.k3s.io | K3S_URL=https://104.248.61.65:6443 K3S_TOKEN=K102cf6b51d3e1bca9832acab7ac7bb385c0ce13a186c787f2494dbb3dbedf82dc7::server:d9223d01ef6fc7bb5cf42f52d7921339 sh -

Instalar o K3S e realizar o JOIN ma máquina k8s-agent-3:

``` bash
#acessar a máquina virtual
ssh root@ipDoAgentK8sAgent3

#executar o comando
curl -sfL https://get.k3s.io | K3S_URL=https://IpDoServer:6443 K3S_TOKEN=TOKEN sh -
```

## Arquivo de configuração

Visualizar o arquivo de configuração na máquina k8s-server-1:
``` bash
cat /etc/rancher/k3s/k3s.yaml
``

No bash da máquina local copiar o arquivo de configuração:

``` bash
scp root@iPdoServer:/etc/rancher/k3s/k3s.yaml /home/user/.kube/config
```

No arquivo config salvo na máquina local, alterar o localhost pelo IP Público da máquina k8s-server-1 gerado pelo Cloud Provider:

``` yaml
clusters
- cluster:
       server: https://ipDoServer:6443
```

Checar os nodes na máquina local:
``` bash
kubectl get nodes
```


## Deployment

Executar deployment na máquina local:

``` bash
# realizar o deploy
kubectl apply -f deployment.yaml 

# checar os objetos do cluster
Kubectl get all

# checar em qual worknodes os pods estão sendo executados
kubectl get pods -o wide
```

Acessar aplicação através do browser:

```
http://ipDoServer:30000
```