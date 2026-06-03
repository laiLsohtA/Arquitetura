# ADR 0003 - Modelo de Comunicação

## Status

Aceito.

## Contexto

No Ciclo 2, foi definida a comunicação por API REST entre o Backend/API Principal e o Serviço de Recomendação com IA. Na Fase 3, essa decisão foi revisada considerando cloud, microsserviços e resiliência.

O ponto principal é escolher entre comunicação síncrona e assíncrona. Como o usuário espera receber recomendações no momento em que acessa a plataforma, a comunicação precisa ser rápida e simples.

## Decisão

A decisão foi manter a comunicação **síncrona via API REST** entre o Backend e o Serviço de IA para o fluxo principal de recomendação.

Também foi definido que a arquitetura poderá usar comunicação assíncrona no futuro para tarefas secundárias, como atualização de histórico, geração de métricas, treinamento do modelo ou processamento em lote.

## Justificativa

A comunicação síncrona foi mantida porque combina melhor com o caso de uso principal: o usuário solicita recomendações e espera receber a resposta naquele momento.

API REST também é uma escolha simples, bastante conhecida e fácil de documentar. Para um projeto acadêmico e para uma primeira versão da arquitetura, ela facilita a compreensão e a implementação.

Por outro lado, essa decisão exige cuidados, porque o Backend fica dependendo da resposta do Serviço de IA. Por isso, ela deve ser usada junto com timeout, cache, fallback e circuit breaker.

## Alternativas consideradas

### Comunicação assíncrona com fila

Foi considerada uma comunicação com fila de mensagens. Essa alternativa seria interessante para reduzir acoplamento e melhorar a tolerância a falhas, porque o Backend não precisaria esperar a resposta imediata do Serviço de IA.

Mesmo assim, ela foi rejeitada para o fluxo principal porque não combina tão bem com a necessidade de recomendação em tempo real. O usuário poderia ter que esperar mais ou consultar depois.

### gRPC

Também poderia ser usado gRPC, principalmente por desempenho em comunicação entre serviços. Porém, ele pode ser menos simples para documentação e testes iniciais, principalmente em um projeto acadêmico.

Foi rejeitado neste momento para manter a solução mais simples.

### REST síncrono

Foi a alternativa escolhida por ser mais simples, direta e suficiente para a necessidade atual. A decisão só é segura porque vem acompanhada dos padrões de resiliência definidos no ADR 0002.

## Trade-offs

### Pontos positivos

- Mais simples de implementar.
- Fácil de testar e documentar.
- Combina com recomendações em tempo real.
- Mantém separação entre Backend e Serviço de IA.
- Facilita entendimento da arquitetura pela equipe.

### Pontos negativos

- O Backend depende da resposta da IA.
- Pode haver aumento de latência.
- Falhas no Serviço de IA podem afetar a experiência do usuário.
- Exige timeout, fallback e circuit breaker.

## Impacto nos RNFs

- **Performance:** atende ao fluxo em tempo real, desde que a IA responda rápido.
- **Confiabilidade:** depende dos mecanismos de resiliência.
- **Manutenibilidade:** boa, pois mantém contrato claro entre os serviços.
- **Escalabilidade:** boa, pois o Serviço de IA pode escalar separadamente.
- **Segurança:** precisa de autenticação e controle no API Gateway.

## Consequências

A comunicação REST síncrona continua sendo a melhor escolha para a versão atual. A arquitetura fica simples e objetiva, mas precisa ser acompanhada de monitoramento e fallback para não prejudicar o usuário em caso de falha.
