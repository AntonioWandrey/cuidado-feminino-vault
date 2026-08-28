---
data: 2026-03-19
atualizado: 2026-08-27
status: aceito
tags:
  - decisao
  - arquitetura
  - backend
---

# ADR-001 — Java 21 + Spring Boot 4

## Contexto

O backend exige uma stack relacional, validada, familiar ao curso e adequada a APIs REST. O código real usa Java 21 e Spring Boot 4.0.4.

## Decisão

Adotar Java 21 LTS e Spring Boot 4 como base do backend, com Spring Data JPA, Validation, Web MVC, Actuator e Flyway.

O backend permanece como capacidade de conteúdo, queixas e evolução dos ciclos. A Entrega 2 de 28/08/2026 não depende dele: o calendário atual é local-first conforme o [[ADR-005 - Calendario local-first e previsao no dispositivo]].

## Consequências

### Positivas

- suporte a Java 21 e ecossistema maduro;
- integração consistente com JPA, validação e migrations;
- alinhamento com o contexto acadêmico;
- separação clara entre controller, service, repository e DTO.

### Negativas e riscos

- Spring Boot 4 exige Jakarta EE atual e dependências recentes;
- tempo de inicialização e consumo maiores que stacks mínimas;
- APIs de ciclo podem divergir do cálculo local enquanto não houver contrato de sincronização;
- o backend deve continuar testável mesmo quando não participa da demonstração local.

## Alternativas consideradas

| Alternativa | Motivo para não escolher |
|---|---|
| Node.js/Express | Menor alinhamento com o domínio já implementado e com o curso |
| Python/FastAPI | Reescrita sem ganho suficiente para esta fase |
| Quarkus | Ecossistema menos familiar para a equipe |
