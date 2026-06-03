# ADR 0001 - Estratégia de Nuvem e Escalabilidade

## Status

Aceito.

## Contexto

O sistema de recomendação de filmes foi pensado como uma aplicação baseada em microsserviços. Nos ciclos anteriores, já foi definida a separação entre Frontend, Backend/API Principal, Serviço de Recomendação com IA, Banco de Dados e Cache.

Na Fase 3, foi necessário decidir como essa arquitetura poderia ser implantada em nuvem. A principal preocupação é que o Serviço de Recomendação com IA pode exigir mais processamento do que as outras partes do sistema, principalmente quando muitos usuários solicitam recomendações ao mesmo tempo.

## Decisão

A decisão foi utilizar uma estratégia de cloud baseada em **containers e serviços gerenciados**, com preferência por um modelo próximo de **PaaS/CaaS**.

A aplicação será pensada para implantação em uma nuvem como AWS, Azure ou GCP. O Frontend pode ficar em um serviço de hospedagem web, enquanto o Backend e o Serviço de IA seriam executados em containers. O Banco de Dados e o Cache devem usar serviços gerenciados, para diminuir o trabalho manual de infraestrutura.

A escalabilidade principal será **horizontal**, criando novas instâncias dos serviços quando houver aumento de demanda.

## Justificativa

Essa decisão combina com a arquitetura de microsserviços porque cada serviço pode ser implantado e escalado separadamente. Por exemplo, se o Serviço de IA estiver recebendo muitas requisições, ele pode ganhar mais instâncias sem obrigar o crescimento do Frontend ou do Backend inteiro.

Segundo Richards e Ford, decisões arquiteturais precisam considerar os trade-offs entre simplicidade, escalabilidade, desempenho e manutenção. Nesse caso, a nuvem aumenta um pouco a complexidade, mas ajuda bastante na escalabilidade e na disponibilidade.

## Alternativas consideradas

### IaaS com máquinas virtuais

Foi considerada a possibilidade de usar máquinas virtuais diretamente. Essa opção daria mais controle sobre o ambiente, mas também exigiria mais configuração manual, atualização, segurança e monitoramento.

Foi rejeitada porque o projeto não precisa desse nível de controle neste momento.

### Serverless

Também foi considerada uma abordagem serverless. Ela poderia reduzir o custo em cenários de baixo uso e facilitar a escalabilidade automática. Porém, para o Serviço de IA, pode haver limitações de tempo de execução, dependências e controle do ambiente.

Foi rejeitada como opção principal, mas pode ser usada futuramente em tarefas específicas, como processamento assíncrono, notificações ou geração de relatórios.

### PaaS/CaaS com containers

Essa foi a alternativa escolhida porque mantém um bom equilíbrio. A equipe ainda consegue organizar os serviços em containers, mas sem precisar cuidar de toda a infraestrutura manualmente.

## Trade-offs

### Pontos positivos

- Facilita a escalabilidade horizontal.
- Permite separar melhor os serviços.
- Reduz trabalho operacional usando banco e cache gerenciados.
- Ajuda a preparar o projeto para produção.
- Combina bem com microsserviços.

### Pontos negativos

- Exige mais cuidado com custos de cloud.
- Aumenta a necessidade de monitoramento.
- Exige configuração de rede, autenticação e segurança.
- Pode ser mais complexo que uma implantação simples em servidor único.

## Impacto nos RNFs

- **Escalabilidade:** melhora, pois os serviços podem crescer separadamente.
- **Performance:** melhora com cache e distribuição de carga.
- **Manutenibilidade:** melhora porque cada serviço pode evoluir de forma independente.
- **Confiabilidade:** melhora com recursos de monitoramento, logs e recuperação.
- **Segurança:** exige mais configuração, mas permite controles melhores na nuvem.

## Consequências

A arquitetura passa a ser mais preparada para um ambiente real. Por outro lado, a equipe precisa documentar bem os serviços, controlar custos e acompanhar métricas de uso.
