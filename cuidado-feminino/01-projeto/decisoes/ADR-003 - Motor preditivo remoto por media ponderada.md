---
data: 2026-04-15
atualizado: 2026-08-27
status: substituido
substituido_por: ADR-005
tags:
  - decisao
  - arquitetura
  - algoritmo
  - historico
---

# ADR-003 — Motor preditivo remoto por média ponderada

## Contexto histórico

O backend foi planejado para prever o próximo ciclo com uma média ponderada de até 12 registros, dando maior peso aos mais recentes.

## Decisão original

- usar média ponderada dos registros remotos;
- estimar ovulação como próxima menstruação menos 14 dias;
- estimar período fértil de cinco dias antes até um dia depois da ovulação;
- classificar confiança por quantidade de histórico.

## Motivo da substituição

A Entrega 2 passou a ser local-first e adotou outro contrato: média simples dos últimos seis **intervalos entre datas de início**. Além disso, o backend atual alimenta o motor com `duracaoDias`, que representa dias de sangramento, não o intervalo entre ciclos.

Esta decisão permanece como histórico do motor remoto, mas não descreve o algoritmo em uso na aplicação. O comportamento vigente está no [[ADR-005 - Calendario local-first e previsao no dispositivo]].

## Consequência pendente

Antes de reativar o motor remoto ou sincronizar dados, corrigir a semântica da duração, criar testes de regressão e escolher uma única regra de previsão versionada.
