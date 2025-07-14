# Escolha do Ambiente

## Tipo de Ambiente Utilizado

O ambiente escolhido para execução do projeto foi **nuvem pública (Cloud)**, utilizando como provedor a **Amazon Web Services (AWS)**. A aplicação foi implantada em um **cluster Kubernetes gerenciado via Amazon EKS (Elastic Kubernetes Service)**.

## Justificativa da Escolha

A escolha da AWS como ambiente principal foi motivada pelo objetivo de **simular um cenário real de produção em empresas que utilizam computação em nuvem** para hospedar e escalar suas aplicações. A AWS oferece um conjunto robusto de serviços gerenciados, integração nativa com Kubernetes via EKS, além de uma camada gratuita (Free Tier) ideal para testes e simulações.

Além disso, o uso de Kubernetes na nuvem facilita a orquestração dos contêineres da aplicação, oferecendo alta disponibilidade, balanceamento de carga, escalabilidade automática e facilidade na integração com práticas de GitOps.

## Descrição das Instâncias Criadas

Foram utilizadas **5 instâncias EC2 do tipo `t3.micro`**, compatíveis com o plano **AWS Free Tier**, para a composição dos nós do cluster Kubernetes.

| Função         | Tipo de Instância | vCPU | Memória | Sistema Operacional |
|----------------|-------------------|------|---------|----------------------|
| Nó do cluster  | t3.micro          | 2    | 1 GB    | Amazon Linux 2       |
| Total de nós   | 5 instâncias      | —    | —       | —                    |

Todas as instâncias foram provisionadas como **nós de trabalho (worker nodes)** do cluster EKS. A gestão do plano de controle (control plane) é feita pela própria AWS como parte do serviço EKS, o que reduz a complexidade de configuração e manutenção.

Esse setup se mostrou suficiente para o escopo do projeto e para demonstrar a viabilidade de uma aplicação em contêineres rodando em um ambiente cloud-based real.
