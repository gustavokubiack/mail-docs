# Provisionamento

## Ferramenta Utilizada

Para o provisionamento da infraestrutura, foi utilizada a ferramenta **Terraform**, uma das mais robustas e populares soluções de Infrastructure as Code (IaC). O Terraform permite definir, versionar e aplicar configurações de infraestrutura de forma declarativa, com total controle e auditabilidade.

## Estrutura de Provisionamento

A seguir, uma descrição das principais partes do código Terraform utilizado:

### 🔸 VPC e Subnets

Foi criada uma **VPC personalizada** com suporte a DNS e hostname, subdividida em:

- **Subnets públicas:** destinadas ao cluster EKS.
- **Subnets privadas:** reservadas para o banco de dados PostgreSQL no RDS.

Também foi configurado um **Internet Gateway** e uma **Route Table** para roteamento externo das subnets públicas.

### 🔸 IAM Roles e Policies

Duas roles principais foram configuradas:

- **eks_cluster_role**: utilizada pelo plano de controle do EKS com a policy `AmazonEKSClusterPolicy`.
- **eks_node_role**: atribuída aos nós do cluster com as seguintes policies:
  - `AmazonEKSWorkerNodePolicy`
  - `AmazonEKS_CNI_Policy`
  - `AmazonEC2ContainerRegistryReadOnly`

### 🔸 Cluster EKS

O cluster Kubernetes foi criado com o nome `mail-cluster`, configurado para usar as subnets públicas da VPC, com autenticação baseada em IAM Roles.

### 🔸 Node Group

Foi criado um grupo de nós com:
- **5 instâncias EC2 `t3.micro`**
- **Auto scaling fixo (min = 1, max = 5, desired = 5)**
- Tipo de instância compatível com o Free Tier da AWS.

Esses nós foram associados à role `eks_node_role` para comunicação adequada com o EKS e demais serviços.

### 🔸 Banco de Dados (RDS)

Um banco de dados **PostgreSQL** foi provisionado usando `aws_db_instance` com:
- **20 GB de armazenamento**
- Instância `db.t3.micro`
- Subnets privadas e um **Security Group** configurado para permitir apenas acesso interno pela VPC (porta 5432).

### 🔸 Helm Release (ArgoCD)

O ArgoCD foi instalado via Helm no cluster EKS. A release foi definida como `argocd`, utilizando o repositório oficial do projeto Argo Helm:

- Namespace: `argocd`
- Chart: `argo-cd`
- Versão: `5.46.7`
- Arquivo customizado: `argocd-values.yaml`

### 🔸 Kubernetes Secrets

Foi criado um `Secret` Kubernetes com todas as variáveis sensíveis da aplicação, como:

- Dados do banco (host, user, senha)
- Configurações de e-mail
- Flags de debug
- Informações de autenticação SMTP

## Desafios e Soluções

### 🔸 Capacidade dos Nós e Limite de Pods

Inicialmente, o cluster foi configurado com **apenas 1 nó `t3.micro`**, o que rapidamente gerou problemas de capacidade. Isso se deve ao fato de que:

- A AWS executa **pods internos no cluster**, como o `CoreDNS`, `kube-proxy`, `aws-node`, entre outros.
- A instância `t3.micro` tem um limite baixo de **pods por nó** (aproximadamente 11 a 17, dependendo da configuração da ENI).
- A aplicação, ArgoCD e serviços auxiliares também consomem pods.

### 🔧 Solução

Para resolver, foi realizado o **aumento do número de nós de 1 para 5**, garantindo maior disponibilidade de recursos para os pods do sistema e da aplicação, além de permitir uma operação mais próxima da realidade de produção.

Esse ajuste resolveu os problemas de scheduling e permitiu a instalação e operação estável do ArgoCD e da aplicação.

---

### Vídeo de Demonstração

<iframe width="560" height="315" src="https://www.youtube.com/embed/4deqtnkoYVE?si=Go64BnngdnCZ4SYl" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
