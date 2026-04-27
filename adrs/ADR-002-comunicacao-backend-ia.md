# ADR 002 – Adoção de API REST para Comunicação entre Backend e Serviço de IA

## Status
Aceito

## Contexto
O sistema de recomendação de filmes é composto por diferentes containers, incluindo o Frontend, Backend Principal, Banco de Dados e Serviço de Recomendação com IA. Como o serviço de IA foi separado da plataforma principal, tornou-se necessário definir uma estratégia de comunicação entre o Backend e o Serviço de IA.

## Decisão
Foi decidido utilizar comunicação síncrona por meio de API REST entre o Backend Principal e o Serviço de Recomendação com IA.

## Justificativa
A API REST foi escolhida por ser simples, amplamente utilizada e de fácil integração. Como o usuário espera receber recomendações em tempo real, a comunicação síncrona é adequada.

## Trade-offs
### Vantagens
- Simples de implementar e documentar.
- Facilita a integração entre Backend e Serviço de IA.
- Permite comunicação direta para recomendações em tempo real.
- Mantém o Serviço de IA independente da plataforma principal.

### Desvantagens
- Pode gerar latência se o Serviço de IA demorar para responder.
- Cria dependência direta entre Backend e Serviço de IA.
- Exige tratamento de falhas, timeout e fallback.

## Alternativa Rejeitada
Foi considerada uma comunicação assíncrona por fila de mensagens. Porém, essa alternativa foi rejeitada neste momento porque aumentaria a complexidade da arquitetura e não seria ideal para recomendações solicitadas em tempo real pelo usuário.

## Impacto nos RNFs
A decisão impacta positivamente a manutenibilidade e a simplicidade da integração. Também atende ao requisito de performance, desde que sejam definidos limites de timeout, cache e fallback.