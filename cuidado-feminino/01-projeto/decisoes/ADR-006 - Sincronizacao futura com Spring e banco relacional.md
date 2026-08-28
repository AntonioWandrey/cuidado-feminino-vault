---
data: 2026-08-27
status: proposto
tags:
  - decisao
  - arquitetura
  - sincronizacao
  - backend
  - lgpd
---

# ADR-006 — Sincronização futura com Spring e banco relacional

## Contexto

Hoje existem duas capacidades separadas: calendário local no cliente e endpoints de ciclo no backend. Não há autenticação, consentimento, identidade estável nem política de conflitos. Os motores de previsão também divergem.

## Decisão proposta

Não implementar sincronização na Entrega 2. Antes de ligar o calendário local à API:

1. corrigir a semântica de duração do ciclo no backend;
2. definir um contrato versionado para registros de período;
3. implementar autenticação e isolamento por usuária;
4. solicitar consentimento específico para sincronização;
5. escolher fonte de verdade e política de conflito;
6. definir exportação, exclusão e auditoria;
7. executar migração local e testes ponta a ponta.

Uma abordagem baseada em eventos de ciclo com identificador estável por registro é a candidata inicial, mas ainda não está aprovada.

## Consequências

- evita divergência silenciosa entre local e servidor;
- mantém a entrega offline simples;
- adia backup e multi-dispositivo;
- transforma autenticação e LGPD em pré-requisitos reais da sincronização.

## Alternativas consideradas

| Alternativa | Avaliação |
|---|---|
| Sincronização bidirecional imediata | Alto risco de conflito e vazamento sem identidade/consentimento |
| Manter local-only permanentemente | Simples, mas sem recuperação, análise longitudinal ou multi-dispositivo |
