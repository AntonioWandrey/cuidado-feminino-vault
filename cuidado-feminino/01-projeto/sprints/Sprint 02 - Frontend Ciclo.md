---
tags:
  - sprint
  - frontend
  - calendario
  - ciclo
status: concluido-com-escopo-replanejado
inicio: 2026-04-20
conclusao: 2026-08-27
---

# Sprint 02 — Frontend do ciclo menstrual

## Resultado

A sprint original previa integração obrigatória com `/api/v1/ciclos`. Para atender à entrega acadêmica local e reduzir dependência de infraestrutura, o escopo foi replanejado para calendário local-first, conforme o [[ADR-005 - Calendario local-first e previsao no dispositivo]].

## Concluído

- [x] calendário mensal com fases previstas;
- [x] registro de início e término do período;
- [x] destaque de períodos reais abertos e fechados;
- [x] histórico completo e expansível;
- [x] previsão do próximo ciclo no cliente;
- [x] persistência local após recarga;
- [x] integração visual com registro de queixas;
- [x] métricas de regularidade no perfil;
- [x] testes unitários e de componente;
- [x] build Web e roteiro de demonstração.

## Replanejado para backlog

- [ ] sincronizar ciclos com o backend;
- [ ] resolver conflitos local/remoto;
- [ ] autenticação e multiusuário;
- [ ] armazenamento criptografado;
- [ ] relatório clínico e exportação;
- [ ] insights avançados por fase.

Esses itens dependem do [[ADR-006 - Sincronizacao futura com Spring e banco relacional]].

## Critério de aceite alcançado

```text
registrar início
→ mostrar período aberto
→ encerrar período
→ consultar histórico
→ calcular previsão
→ recarregar
→ manter dados no dispositivo
```
