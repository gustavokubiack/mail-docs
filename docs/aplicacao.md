# Aplicação

## Descrição da Aplicação

A aplicação desenvolvida tem como objetivo simular o cadastro de um usuário interessado em um serviço de e-mail. Ao realizar o cadastro, o sistema armazena os dados do usuário em um banco de dados e envia uma confirmação de inscrição por e-mail.

Este fluxo representa uma funcionalidade comum em sistemas de marketing, newsletters ou plataformas SaaS que exigem confirmação de cadastro.

## Tecnologias Utilizadas

A stack da aplicação foi composta por tecnologias modernas, divididas entre backend, frontend e banco de dados:

### 🔹 Backend

- **Django**: Framework web em Python, responsável pela lógica de negócios e administração.
- **Django REST Framework (DRF)**: Utilizado para criar uma API RESTful, permitindo a comunicação entre frontend e backend.
- **PostgreSQL**: Banco de dados relacional utilizado para persistência dos dados de cadastro.

### 🔹 Frontend

- **Vue.js**: Framework progressivo JavaScript para construção de interfaces reativas.
- **Vite**: Ferramenta de build moderna, que oferece carregamento extremamente rápido e suporte ao ecossistema Vue 3.

## Estrutura de Deploy

O deploy da aplicação foi definido de forma declarativa com manifests Kubernetes organizados via **Kustomize**. Os arquivos YAML foram separados por serviços:

```bash
└── k8s/
    ├── backend/
    │   ├── deployment.yaml
    │   ├── service.yaml
    │   └── kustomization.yaml
    ├── frontend/
    │   ├── deployment.yaml
    │   ├── service.yaml
    │   └── kustomization.yaml
    └── kustomization.yaml
```

- Kustomization raiz (`k8s/kustomization.yaml`) unifica ambos os ambientes, permitindo aplicação conjunta via ArgoCD.

## Acesso à Aplicação no Cluster

A aplicação está exposta ao público utilizando um **serviço do tipo LoadBalancer**, o que permite o acesso externo à aplicação.