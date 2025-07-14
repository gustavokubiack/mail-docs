# Cluster Kubernetes

## Ferramenta Utilizada para Instalação

A instalação e gerenciamento do cluster Kubernetes foram realizados por meio do serviço gerenciado **Amazon Elastic Kubernetes Service (EKS)**, que simplifica o processo de criação, atualização e escalabilidade de clusters Kubernetes na nuvem AWS.

O EKS elimina a necessidade de instalação manual do plano de controle (control plane), oferecendo alta disponibilidade e segurança nativa, com integração com diversos serviços da AWS (IAM, VPC, CloudWatch, ELB, etc.).

## Configuração dos Nós

### Estrutura

O cluster foi configurado com:

- **5 nós de trabalho (worker nodes)**  
- **0 nós de controle (control plane gerenciado pelo EKS)**

Todos os nós foram instanciados como EC2 do tipo `t3.micro`, compatíveis com o Free Tier da AWS.

### Configurações Relevantes

- **Auto Scaling Manual:**  
  O grupo de nós foi configurado com um tamanho fixo de 5 instâncias (min, max e desired iguais a 5), para evitar flutuações no ambiente de simulação.

- **Subnets Públicas:**  
  Os nós foram alocados em duas zonas de disponibilidade (`us-east-1a` e `us-east-1b`) em subnets públicas, facilitando o acesso a serviços e ferramentas como ArgoCD.

- **IAM Role com Policies Necessárias:**  
  Cada nó recebeu permissões específicas via IAM Role para:
  - Comunicação com o EKS (`AmazonEKSWorkerNodePolicy`)
  - Gerenciamento de rede (`AmazonEKS_CNI_Policy`)
  - Acesso ao ECR (`AmazonEC2ContainerRegistryReadOnly`)

- **Cluster Security:**  
  A comunicação entre os nós e o plano de controle foi protegida por autenticação baseada em IAM. Além disso, um Security Group limitado foi utilizado para o banco de dados.

## Validação do Funcionamento do Cluster

Após a criação do cluster, foram realizados diversos testes para garantir que o ambiente estava funcional:

### 🔍 Verificação de Infraestrutura

- **EC2 Dashboard (AWS Console):**  
  Verificação da criação correta das 5 instâncias `t3.micro`, distribuídas entre duas zonas de disponibilidade.

- **Subnets e Internet Gateway:**  
  Checagem das subnets públicas atribuídas aos nós, e do roteamento correto via Internet Gateway.

- **Verificação do Cluster via CLI:**

```bash
# Verificar contexto atual
kubectl config get-contexts

# Verificar status dos nós
kubectl get nodes

# Verificar status dos pods do sistema
kubectl get pods -n kube-system

# Verificar status dos pods do ArgoCD
kubectl get pods -n argocd

# Verificar status dos pods da aplicação
kubectl get pods -n default
```
