# ADR 0002 - Padrões de Resiliência

## Status

Aceito.

## Contexto

O Backend/API Principal depende do Serviço de Recomendação com IA para entregar recomendações personalizadas. Como a comunicação acontece por API REST, o Backend envia uma requisição e aguarda a resposta da IA.

O problema é que, em produção, o Serviço de IA pode ficar lento, falhar ou ficar sobrecarregado. Se o Backend depender totalmente dele, o usuário pode ficar sem resposta ou esperar mais do que o tempo aceitável.

No Ciclo 1, o requisito de performance definiu que as recomendações deveriam retornar em menos de 3 segundos. Por isso, a arquitetura precisa prever estratégias de resiliência.

## Decisão

A decisão foi aplicar um conjunto de padrões de resiliência:

- **Timeout** para limitar o tempo de espera do Backend;
- **Retry controlado** para tentar novamente apenas em falhas temporárias;
- **Circuit Breaker** para bloquear chamadas quando o Serviço de IA estiver falhando muito;
- **Fallback** para mostrar filmes populares ou recomendações em cache;
- **Cache Redis** para reduzir chamadas repetidas ao Serviço de IA;
- **API Gateway** para centralizar acesso, segurança e limite de requisições.

## Justificativa

Essa decisão foi tomada porque apenas separar a aplicação em microsserviços não garante que ela será confiável. Na prática, quanto mais serviços existem, mais pontos de falha aparecem.

O Circuit Breaker ajuda a evitar que uma falha em um serviço derrube o restante da aplicação. Se o Serviço de IA começar a falhar, o Backend não fica insistindo indefinidamente. Em vez disso, ele passa a usar uma alternativa, como cache ou lista de filmes populares.

Essa ideia se relaciona com o que autores de arquitetura de software defendem sobre tolerância a falhas: sistemas distribuídos precisam aceitar que falhas vão acontecer e devem estar preparados para responder de forma controlada.

## Alternativas consideradas

### Não usar padrão de resiliência

Seria mais simples, mas deixaria o sistema muito frágil. Qualquer lentidão no Serviço de IA poderia afetar diretamente o usuário.

Foi rejeitada porque não atende bem aos RNFs de performance e confiabilidade.

### Usar apenas retry

Retry pode resolver falhas pequenas e temporárias, mas se for usado sozinho pode piorar o problema. Se o Serviço de IA já estiver sobrecarregado, muitas tentativas podem aumentar ainda mais a carga.

Foi rejeitada como solução única.

### Usar fila assíncrona para tudo

Uma fila poderia aumentar a resiliência, mas não combina totalmente com recomendações em tempo real, pois o usuário espera a resposta na hora.

Foi rejeitada como abordagem principal, mas pode ser usada no futuro para tarefas que não precisam ser imediatas.

## Trade-offs

### Pontos positivos

- Evita que o Backend fique preso esperando a IA.
- Melhora a experiência do usuário em caso de falha.
- Reduz a carga do Serviço de IA com cache.
- Ajuda a manter o sistema funcionando mesmo com problemas parciais.
- Facilita o monitoramento de falhas.

### Pontos negativos

- Aumenta a complexidade da implementação.
- Exige definir bem limites de timeout e retry.
- O fallback pode entregar recomendações menos personalizadas.
- O cache precisa ter tempo de expiração para não ficar desatualizado.

## Impacto nos RNFs

- **Performance:** melhora, pois evita esperas longas.
- **Confiabilidade:** melhora bastante com fallback e circuit breaker.
- **Escalabilidade:** melhora indiretamente, pois o cache reduz carga.
- **Manutenibilidade:** exige mais organização, mas deixa o comportamento do sistema mais previsível.
- **Segurança:** o API Gateway ajuda a aplicar regras de acesso e controle.

## Consequências

A arquitetura fica mais robusta para produção. Mesmo que o Serviço de IA falhe, o sistema ainda consegue responder ao usuário. A recomendação pode não ser tão personalizada no fallback, mas é melhor do que deixar o usuário sem resposta.
