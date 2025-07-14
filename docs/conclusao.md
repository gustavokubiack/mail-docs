# Conclusão

## Lições Aprendidas

Este projeto foi essencial para consolidar conhecimentos práticos em **integração contínua, entrega contínua (CI/CD)** e **GitOps**, além de proporcionar uma compreensão mais profunda da orquestração de aplicações em ambientes Kubernetes gerenciados.

Foi possível aplicar conceitos de **Infrastructure as Code (IaC)** com Terraform, provisionar clusters escaláveis na AWS via EKS, e integrar o controle declarativo de aplicações com ArgoCD. O ciclo completo — da infraestrutura ao deploy automatizado via Git — refletiu cenários reais de produção, o que permitiu aplicar esse aprendizado no ambiente profissional de forma prática e imediata.

## Dificuldades Encontradas

A principal dificuldade técnica enfrentada esteve relacionada à **capacidade insuficiente dos nós do cluster Kubernetes**. Inicialmente, apenas uma instância EC2 do tipo `t3.micro` foi configurada, o que resultou em problemas de agendamento de pods.

A limitação surgiu devido à quantidade de pods padrão consumidos por serviços internos do EKS, como:

- `aws-node`
- `kube-proxy`
- `coredns`
- Componentes do ArgoCD

Esses pods consomem slots que, em instâncias pequenas, comprometem o espaço para os containers da aplicação. Foi necessário escalar manualmente o número de nós para 5 para garantir a estabilidade do cluster. A identificação dessa causa e o ajuste da infraestrutura consumiram tempo significativo durante o desenvolvimento.

## O que Faria Diferente

Se fosse recomeçar o projeto, uma melhoria fundamental seria a **configuração de um domínio personalizado**. Durante o desenvolvimento, o acesso à aplicação foi feito diretamente por meio da URL gerada pelo **Elastic Load Balancer (ELB)** da AWS, o que não é ideal para ambientes de produção.

Essa melhoria aumentaria a aderência do projeto a práticas reais de deploy e operação de sistemas em produção.

---
