---
data: 2026-04-15
sprint: Sprint 01
tags:
  - git
  - dev
  - sprint-01
---

# Git Journal - 2026-04-15 (Conclusao Sprint 01)

## Commits do Sprint 01

| Hash | Descricao | Layer |
|------|-----------|-------|
| bcd7d69 | feat(ciclo): implementa motor preditivo de ciclo menstrual (Sprint 01) | domain/service |
| e387a75 | feat: finaliza integracao completa, corrige layout e persiste dados no mysql | fullstack |
| ae44872 | feat: ajusta layout do check-up, corrige scroll e integra axios com backend | frontend |
| b3084ef | feat: estrutura base do monorepo com frontend limpo e backend inicial | infra |
| 5f62668 | Estrutura inicial: Frontend React e Backend Spring Boot | infra |
| caf6a9b | Step 3: Upgrade Java Version - Compile: SUCCESS | backend |
| d870dc3 | Step 1: Setup Environment - Environment setup complete | infra |
| 9d154b2 | chore(frontend): remove artefatos do Lovable e dependencias nao utilizadas | frontend |
| 647bde5 | chore(infra): adiciona CLAUDE.md e limpa comentario gerado no index.css | infra |

> **Nota:** Commits "Step 1/Step 3" nao seguem Conventional Commits — foram gerados automaticamente. Monitorar para nao se repetirem.

---

## O que foi Implementado no Sprint 01

- MotorPreditivoCiclo com media movel ponderada (12 ciclos) e fallback de 28 dias
- CicloService com transacoes e validacoes de negocio
- CicloController com 5 endpoints: POST /ciclos, PATCH /ciclos/{id}/encerrar, GET /ciclos, GET /ciclos/previsao, GET /ciclos/calendario
- GlobalExceptionHandler com CicloNaoEncontradoException (404) e CicloEmAbertoException (409)
- Migration V2 com tabelas ciclo_menstrual e previsao_ciclo

---

## Decisoes Tecnicas Tomadas

- MotorPreditivoCiclo marcado como @Component (nao @Service) — e um componente de calculo puro, nao orquestra regras de negocio
- usuario_id fixado como 1L em CicloService ate Sprint 04 (JWT)
- desvio_padrao retorna null quando menos de 2 ciclos — BigDecimal nullable intencional
- CalendarioResponse usa projecao simplificada — cada ciclo projetado dura DURACAO_MEDIA_CICLO_DIAS (5 dias)

---

## Dividas Tecnicas Geradas

- Flyway desabilitado — ddl-auto=update em uso
- Credenciais hardcoded (root/root)
- show-sql=true ativo
- api.ts no frontend aponta para /api/saude (Sprint 00) em vez de /api/v1

---

## Aprendizados

- Spring Boot 4.x usa Jakarta EE 11 (`jakarta.*` em vez de `javax.*`) — importante para imports
- @PrePersist e @PreUpdate sao hooks de ciclo de vida do JPA executados automaticamente antes de INSERT e UPDATE
- Records Java sao imutaveis — ideal para DTOs de request/response sem boilerplate
- @Transactional(readOnly = true) otimiza queries de leitura no Hibernate

## Adendo — 27/08/2026

Este registro é histórico. Flyway foi reativado posteriormente e o frontend deixou de depender de uma `baseURL` fixa por módulo. Para a entrega preliminar, o calendário passou a usar persistência e previsão locais conforme o [[ADR-005 - Calendario local-first e previsao no dispositivo]].
