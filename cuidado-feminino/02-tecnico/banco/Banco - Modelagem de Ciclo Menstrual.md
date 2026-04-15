---
tags:
  - banco
  - modelagem
  - sprint-01
---

# Banco - Modelagem de Ciclo Menstrual

> Decisoes de modelagem para as tabelas de ciclo e previsao.
> Atualizado em: 2026-04-15 — V2 implementada, Flyway desabilitado (ver dividas tecnicas)

---

## Status das Migrations

| Versao | Arquivo | Status |
|--------|---------|--------|
| V1 | V1__initial_schema.sql | ✅ Criada — tabela `registros_saude` |
| V2 | V2__create_cycle_tables.sql | ✅ Criada — `ciclo_menstrual` + `previsao_ciclo` |

> **Atencao:** `spring.flyway.enabled=false` — o schema e gerenciado por `ddl-auto=update`.
> As migrations existem no classpath mas nao estao sendo executadas pelo Flyway.
> Reativar Flyway e uma divida tecnica prioritaria antes do Sprint 04 (JWT + producao).

---

## Diagrama de Entidades

```
usuario (Sprint 04 - JWT)
    |
    |--< ciclo_menstrual
    |       id              BIGINT PK AUTO_INCREMENT
    |       usuario_id      BIGINT NOT NULL DEFAULT 1 (fixo ate Sprint 04)
    |       data_inicio     DATE NOT NULL
    |       data_fim        DATE NULL (null enquanto ciclo esta aberto)
    |       duracao_dias    INT NULL  (calculado via @PrePersist/@PreUpdate)
    |       fluxo           ENUM (LEVE, MODERADO, INTENSO, MUITO_INTENSO)
    |       observacoes     TEXT
    |       criado_em       DATETIME NOT NULL
    |       atualizado_em   DATETIME
    |
    |--< previsao_ciclo
            id                      BIGINT PK AUTO_INCREMENT
            usuario_id              BIGINT NOT NULL DEFAULT 1
            gerada_em               DATETIME NOT NULL
            media_duracao_ciclo     DECIMAL(5,2)
            desvio_padrao           DECIMAL(5,2) NULL
            ciclos_analisados       INT
            proxima_menstruacao     DATE
            data_ovulacao           DATE
            inicio_periodo_fertil   DATE
            fim_periodo_fertil      DATE
            confianca               ENUM (BAIXA, MEDIA, ALTA)
```

---

## Decisoes de Modelagem

### Por que data_fim e nullable?
Um ciclo comeca quando a menstruacao inicia. A usuaria registra o inicio agora e encerra depois. NULL em data_fim indica ciclo em aberto. O metodo `estaAberto()` na entity encapsula essa logica.

### Por que duracao_dias e calculado e salvo?
Evita recalcular em todas as consultas do motor preditivo. O `@PreUpdate` do JPA calcula automaticamente: `duracao = DAYS.between(dataInicio, dataFim) + 1`.

### Por que usar ENUM no banco?
Para fluxo e confianca com valores fixos, ENUM e mais simples que FK para tabela de dominio e garante integridade. Java enum mapeado com `@Enumerated(EnumType.STRING)`.

### Por que salvar previsao_ciclo e nao calcular on-the-fly?
- Historico de previsoes para auditoria clinica
- Performance: nao recalcula a cada request
- Futuro: permite comparar previsao x realidade para melhorar o algoritmo

### Por que usuario_id = 1 por enquanto?
JWT e autenticacao estao planejados para Sprint 04. Ate la, todas as operacoes usam `USUARIO_ID_PADRAO = 1L` fixo no `CicloService`.

---

## Indices (V2)

```sql
-- Indice composto para busca de ciclos por usuario
INDEX idx_ciclo_usuario (usuario_id)
INDEX idx_ciclo_data_inicio (data_inicio)

-- Indice para ultima previsao gerada
INDEX idx_previsao_usuario (usuario_id)
```

> Os indices do V2 sao simples. Quando Flyway for reativado, pode-se criar V3 com indices compostos otimizados:
> ```sql
> CREATE INDEX idx_ciclo_usuario_inicio ON ciclo_menstrual(usuario_id, data_inicio DESC);
> CREATE INDEX idx_previsao_usuario_gerada ON previsao_ciclo(usuario_id, gerada_em DESC);
> ```

---

## Queries Uteis para DataGrip

```sql
-- Todos os ciclos de uma usuaria
SELECT * FROM ciclo_menstrual WHERE usuario_id = 1 ORDER BY data_inicio DESC;

-- Ultima previsao gerada
SELECT * FROM previsao_ciclo WHERE usuario_id = 1 ORDER BY gerada_em DESC LIMIT 1;

-- Ciclos completos com duracao
SELECT data_inicio, data_fim, duracao_dias, fluxo
FROM ciclo_menstrual
WHERE usuario_id = 1 AND data_fim IS NOT NULL
ORDER BY data_inicio DESC;

-- Media real de duracao (validar contra o motor)
SELECT AVG(duracao_dias) as media, STDDEV(duracao_dias) as desvio
FROM ciclo_menstrual
WHERE usuario_id = 1 AND duracao_dias IS NOT NULL;

-- Ciclo em aberto atualmente
SELECT * FROM ciclo_menstrual
WHERE usuario_id = 1 AND data_fim IS NULL
ORDER BY data_inicio DESC LIMIT 1;
```
