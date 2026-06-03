# Gold Plating - Plano de Resiliência

Este material foi criado como complemento da entrega para mostrar como a arquitetura poderia se comportar em cenários de falha.

## Cenário 1 - Serviço de IA lento

**Problema:** o Serviço de Recomendação com IA demora mais que 3 segundos para responder.

**Resposta da arquitetura:**

1. O Backend aplica timeout.
2. A requisição para a IA é interrompida.
3. O Backend consulta o Cache Redis.
4. Se houver recomendação recente, ela é retornada.
5. Se não houver cache, o sistema retorna uma lista de filmes populares.

## Cenário 2 - Serviço de IA fora do ar

**Problema:** o Serviço de IA falha várias vezes seguidas.

**Resposta da arquitetura:**

1. O Circuit Breaker identifica muitas falhas.
2. O circuito é aberto temporariamente.
3. O Backend para de chamar o Serviço de IA por um período.
4. O fallback assume a resposta.
5. O monitoramento registra alertas para a equipe.

## Cenário 3 - Pico de acessos

**Problema:** muitos usuários pedem recomendações ao mesmo tempo.

**Resposta da arquitetura:**

1. O API Gateway controla a entrada das requisições.
2. O Cache reduz chamadas repetidas.
3. O Serviço de IA escala horizontalmente.
4. Logs e métricas ajudam a acompanhar o comportamento do sistema.

## Por que isso é melhoria arquitetural?

Porque não é apenas uma alteração de texto. A proposta muda o comportamento esperado da arquitetura em produção, adicionando mecanismos concretos para reduzir falhas e manter o sistema respondendo ao usuário.
