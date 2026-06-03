# Gold Plating - Checklist de Observabilidade

Este checklist serve para acompanhar a saúde do sistema em produção.

## Métricas do Backend

- Tempo médio de resposta das APIs.
- Quantidade de erros 4xx e 5xx.
- Quantidade de chamadas ao Serviço de IA.
- Quantidade de respostas vindas do fallback.
- Tempo médio de autenticação e validação.

## Métricas do Serviço de IA

- Tempo médio para gerar recomendações.
- Taxa de erro.
- Uso de CPU e memória.
- Número de requisições simultâneas.
- Quantidade de timeouts.

## Métricas do Cache

- Taxa de acerto do cache.
- Taxa de erro no Redis.
- Tempo médio de resposta do cache.
- Quantidade de recomendações expiradas.

## Alertas sugeridos

- Serviço de IA com erro acima de 10%.
- Tempo de resposta maior que 3 segundos.
- Circuit Breaker aberto por muito tempo.
- Cache indisponível.
- Banco de dados com muitas conexões abertas.

## Logs importantes

- Requisições feitas pelo usuário.
- Chamadas ao Serviço de IA.
- Uso de fallback.
- Abertura e fechamento do Circuit Breaker.
- Falhas de autenticação.
