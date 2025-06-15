# Projeto: Orquestração de aplicações com Kubernetes - Portal de Notícias

---

## Visão geral do projeto

Este projeto é uma demonstração prática do meu conhecimento em **DevOps** e na **orquestração de contêineres utilizando Kubernetes**. Utilizei o **portal de notícias (Projeto disponibilizado pela Alura)** como aplicação de exemplo, mas o foco principal reside na infraestrutura por trás, que ilustra como o Kubernetes pode ser empregado para **gerenciar, escalar e garantir a resiliência de aplicações em ambientes de produção**.

Meu objetivo é apresentar a capacidade de construir e manter uma infraestrutura de aplicação robusta e escalável, utilizando as melhores práticas do Kubernetes.

---

## 🎯 Objetivos de aprendizagem e demonstração

Através deste projeto, demonstro compreenção dos seguintes aspectos do Kubernetes:

* **Criação e gerenciamento de clusters:** Habilidade em configurar e manter um ambiente de orquestração de contêineres.
* **Escalabilidade de aplicações:** Implementação de estratégias para o escalonamento horizontal de recursos, garantindo que a aplicação possa lidar com diferentes demandas de tráfego.
* **Gerenciamento de configurações e variáveis de ambiente:** Utilização de **configMaps** para desacoplar configurações da aplicação do código, facilitando a gestão de variáveis de ambiente e a segurança de dados sensíveis.
* **Provisão de infraestrutura confiável e escalável:** Demonstração da capacidade de construir uma base sólida para aplicações de missão crítica, com foco em alta disponibilidade e capacidade de resposta.

---

## 🛠️ Tecnologias utilizadas

Este projeto foi construído e orquestrado utilizando as seguintes ferramentas e tecnologias chave:

* **Docker:** Essencial para a **criação, empacotamento e execução isolada dos contêineres** da aplicação.
* **Kubernetes:** A plataforma central para a **orquestração, gerenciamento, escalonamento e automação da implantação** dos contêineres.
* **Minikube:** Utilizado para criar um **cluster Kubernetes local** para desenvolvimento e testes, replicando um ambiente de produção em escala reduzida.
* **KVM (Kernel-based Virtual Machine):** Empregado como **solução de virtualização** para o Minikube no Linux, garantindo um ambiente isolado e eficiente para o cluster local.

---

## 🏗️ Arquitetura do projeto

O cluster Kubernetes foi arquitetado para suportar o portal de notícias com os seguintes componentes e padrões:

* **Cluster Kubernetes:** O coração da infraestrutura, onde os contêineres da aplicação são executados e gerenciados.
* **Services:** Definidos para **habilitar a comunicação interna** entre os diferentes componentes da aplicação (ex: frontend para backend) e **expor a aplicação externamente** (acesso ao portal de notícias).
* **ConfigMaps:** Utilizados para **armazenar dados de configuração sensíveis** e variáveis de ambiente, que são injetados nos contêineres da aplicação em tempo de execução, promovendo a flexibilidade e a manutenção.
* **Múltiplos contêineres:** A aplicação é construida em contêineres específicos (ex: frontend, backend, banco de dados), ilustrando a abordagem de microsserviços e a capacidade do Kubernetes de orquestrá-los de forma coesa.

---

## 📋 Pré-requisitos

Para executar este projeto localmente no Linux, você precisará ter as seguintes ferramentas instaladas e configuradas:

* **Git:** Para clonar o repositório.
    * Instalação: `sudo apt update && sudo apt install git -y` (*Utilize o gerenciador de pacotes da sua distribuição linux*)
* **Docker Engine:** Essencial para a construção e execução das imagens de contêiner.
    * [Documentação oficial para Linux](https://docs.docker.com/engine/install/ubuntu/)
* **Kubectl:** A ferramenta de linha de comando para interagir com clusters Kubernetes.
    * [Documentação oficial para Linux](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)
* **Minikube:** Para provisionar e gerenciar o cluster Kubernetes local.
    * [Documentação oficial para Linux](https://minikube.sigs.k8s.io/docs/start/)
* **KVM/QEMU (Opcional, mas recomendado para Minikube no Linux):** Minikube utiliza drivers de virtualização. O KVM é um excelente driver para Linux, mas podem utilizar outros, como exemplo, o virtualbox.
    * Instalação (Ubuntu/Debian): `sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils -y`
    * Adicionar seu usuário ao grupo `libvirt`: `sudo usermod -a -G libvirt $(whoami)` 

---

## Como executar o projeto

Siga os passos abaixo para levantar seu cluster Kubernetes local com Minikube e implantar a aplicação "Portal de Notícias".

### 1. Clonar o repositório

Comece clonando este projeto para sua máquina:
```bash
git clone git@github.com:mizaelZuza/portal-de-noticias.git
cd portal-de-noticias
```

### 2. Iniciar o cluster Minikube

Inicie o Minikube com recursos adequados e um perfil nomeado:
```bash
minikube start --driver=kvm2 --memory=4096mb --cpus=2 --profile portal-de-noticias
```
*A quantidade de memória e cpus pode ser alterada conforme sua necessidade e recurso computacional disponível. Basta alterar as tags `--memory` e `--cpus`*

Confirme se o cluster está ativo:
```bash
minikube status --profile portal-de-noticias
```

### 3. Configurar kubectl

Aponte seu `kubectl` para o cluster recém-criado:

```bash
kubectl config use-context portal-de-noticias
kubectl config current-context
```

### 4. Aplicar manifestos Kubernetes

Implante todos os componentes da aplicação no cluster:

```bash
# Assumindo que vc está no diretório dos arquivos yaml
kubectl apply -f .
```

### 5. Verificar status da aplicação

Confirme que os componentes foram implantados e estão rodando:

```bash
# A tag -o wide é opcional, formata a saida (resultado) com algumas informações extras.
# Informações que são ocultas quando executado o comando sem a tag.
kubectl get pods -o wide
kubectl get services -o wide
kubectl get configmaps
```

### 6. Acessar a aplicação

Obtenha a URL do seu serviço para acessá-lo no navegador:

*### No linux com o minikube o acesso é feito com o ip atribuido ao cluster*

```bash
minikube profile list
```

1.  Copie o IP atribuido ao cluster `portal-de-noticias`;
2.  Cole o IP do cluster, seguido da porta configurada para o portal de noticias [`30000`] Ex: `http://ipdocluster:30000`;
3.  Caso queira acessar o portal de cadastro das noticias, cole o IP seguido da porta configurada para acesso a essa parte da aplicação [`30001`] EX: `http://ipdocluster:30001`.

*O usuário e senha padrão é user: admin e pass: admin*

### 7. Parar ou deletar o cluster

Ao finalizar, você pode pausar ou remover o cluster:

* **Pausar (preserva o estado):**
    ```bash
    minikube stop --profile portal-de-noticias
    ```
* **Deletar (remove tudo):**
    ```bash
    minikube delete --profile portal-de-noticias
    ```