# K3S

K3S indicado para arquitetura Edge , IOT e Pipeline CI/CD.


## Máquinas Virtuais

O Cloud Provider utlizado neste laboratório foi o da Digital Ocean.

Utilizar o recurso de Droplets para criar as máquinas virtuais.

Quantidade necessária para este laboratório:

=> 01 Máquina Virtual irá fazer o papel do Control-Pane.
Minimo para esta máquina 2GB e 1 CPU.

=> No minimo 2 máquinas virturias para fazer o papel de WorkNodes.
Minimo para esta máquina 1GB e 1 CPU.

Nomear os Droplets:
=> k8s-server-1
=> k8s-agent-2
=> k8s-agent-3

Cada Droplet terá um numero de IP.


## Instalar o K3S no Server

Instalar K3s na máquina k8s-server-1:
``` bash
#acessar a máquina virtual
ssh root@ipDoK8s-server-1

# Instalar o k3s
curl -sfL https://get.k3s.io | sh -

# Checar os nodes
Kubectl get nodes
```

Na máquina k8s-server-1 anotar o Token:
``` bash
cat /var/lib/rancher/k3s/server/node-token
```

Sair da máquina:

``` bash
exit
``` 

## Join do Agent com Server:


Instalar o K3S e realizar o JOIN:

``` bash
# acessar a máquina virtual k8s-agent-2
ssh root@ipDoAgentK8sAgent

# executar o comando
# copiar o token do K8s-Server-1 e coloar no comando abaixo
curl -sfL https://get.k3s.io | K3S_URL=https://ipDok8S-server-1:6443 K3S_TOKEN=tokenDoK8s-Server-1 sh -
```

Repetir o processo para cada máquina que irá representar o agent (workernode).


## Arquivo de configuração (Copiar e Colar)

Acessar a máquina k8s-server-1.

Para acessar o kubernetes da sua máquina local é necessário copiar os dados de autenticação, e colar no arquivo ".config" da máquina local.

Primeira forma é visualizar o arquivo de configuração na máquina k8s-server-1:

``` bash
cat /etc/rancher/k3s/k3s.yaml
```

## Arquivo de configuração (via ssh)

Acessar a máquina k8s-server-1.

A outra forma é via ssh na máquina local:

``` bash
scp root@iPdoK8sServer1:/etc/rancher/k3s/k3s.yaml /home/nomeDoUsuarioMaquinaLocal/.kube/config
```

Sair da máquina:
``` bash
exit
```

## Máquina Local

No arquivo config salvo na máquina local, alterar o localhost pelo IP Público da máquina k8s-server-1 gerado pelo Cloud Provider:


Acessar o arquivo config salvo na pasta ".kube":

``` yaml
...
clusters
- cluster:
       server: https://ipDoK8sServer1:6443
...
```

Checar os nodes na máquina local:
``` bash
kubectl get nodes
```