---
data: 2026-03-19
atualizado: 2026-08-27
status: aceito
tags:
  - decisao
  - arquitetura
  - banco
---

# ADR-002 — MySQL compatível com MariaDB

## Contexto

O backend trabalha com registros estruturados e relações entre ciclos, previsões, conteúdos, categorias, séries e queixas. O projeto atual usa MySQL Connector/J, `MySQLDialect` e banco local `cuidado_feminino`.

## Decisão

Usar MySQL como implementação validada no desenvolvimento e manter compatibilidade planejada com MariaDB. Antes de qualquer deploy em MariaDB, todas as migrations Flyway e consultas devem ser executadas nesse banco alvo.

O banco é fonte de verdade dos módulos remotos. Ele não armazena os períodos do calendário local da Entrega 2.

## Consequências

### Positivas

- integração madura com Spring Data JPA e Hibernate;
- ambiente local simples;
- integridade relacional;
- schema versionado por Flyway.

### Negativas e riscos

- “compatível” não significa automaticamente homologado em MariaDB;
- diferenças de versão, tipos e DDL precisam de teste;
- o backend ainda possui limitações single-user;
- sincronização de ciclos exigirá identidade, consentimento e conflitos definidos.

## Estado das migrations

| Versão | Responsabilidade |
|---|---|
| V1 | registros de saúde iniciais |
| V2 | ciclos e previsões |
| V3 | conteúdo educativo, categorias e séries |
| V4 | registro de queixas |

Flyway está presente e deve permanecer ativo nos perfis executáveis. Configurações e credenciais locais não são versionadas.

## Alternativas consideradas

| Alternativa | Motivo para não escolher agora |
|---|---|
| PostgreSQL | Exigiria migração de ambiente sem benefício imediato para a entrega |
| MongoDB | O domínio atual é relacional |
| H2 | Útil para testes isolados, não como banco principal |
