---
data: 2025-01-01
status: aceito
tags:
  - decisao
  - arquitetura
  - banco
---

# ADR-002 - MySQL/MariaDB como Banco Principal

## Contexto

O projeto lida com dados sensiveis de saude feminina estruturados em series temporais. Era necessario um banco relacional confavel, de facil setup local e com boa integracao com Spring Data JPA.

## Decisao

Usar MySQL (com driver `mysql-connector-j`) como banco de dados de desenvolvimento. MariaDB compativel para producao.

> **Estado real:** O projeto usa `mysql-connector-j` (MySQL oficial) e `org.hibernate.dialect.MySQLDialect`.
> O banco local roda em `localhost:3306`, database `cuidado_feminino`.
> MariaDB e compativel e pode ser usado no deploy sem alteracoes no driver.

## Consequencias

**Pros:**
- Totalmente compativel com Spring Data JPA + Hibernate
- Open source e gratuito
- Facil de rodar localmente com MySQL Workbench / DataGrip
- Suporte nativo a tipos DATE, DATETIME e ENUM
- Flyway com `flyway-mysql` compativel

**Contras e Riscos:**
- Nao tem suporte nativo a JSONB como PostgreSQL
- Para escala muito alta, PostgreSQL seria mais indicado
- Flyway atualmente desabilitado — `ddl-auto=update` em uso na dev (divida tecnica)

## Alternativas Consideradas

| Alternativa | Por que foi descartada |
|-------------|------------------------|
| PostgreSQL | MySQL ja estava no ambiente da UNIFEBE |
| MongoDB | Modelo de documento nao se encaixa em dados relacionais de ciclo |
| H2 in-memory | Apenas para testes, nao para producao |

## Estado das Migrations

| Versao | Arquivo | Status |
|--------|---------|--------|
| V1 | V1__initial_schema.sql | Criado — tabela `registros_saude` |
| V2 | V2__create_cycle_tables.sql | Criado — tabelas `ciclo_menstrual` e `previsao_ciclo` |

> **Atencao:** Flyway esta desabilitado (`spring.flyway.enabled=false`).
> O schema esta sendo gerenciado por `ddl-auto=update`.
> Reativar Flyway e uma divida tecnica documentada no [[Home]].

## Referencias

- https://mariadb.com/kb/en/
- https://spring.io/guides/gs/accessing-data-jpa/
