---
data: 2026-04-15
sprint: Sprint 01
tags:
  - sprint
  - retrospectiva
  - sprint-01
---

# Retrospectiva - Sprint 01

> Sprint: Motor Preditivo de Ciclo Menstrual

---

## O que foi Entregue

| Tarefa | Status |
|--------|--------|
| Entidade CicloMenstrual com @PrePersist/@PreUpdate | ✅ |
| Entidade PrevisaoCiclo com enum Confianca | ✅ |
| CicloMenstrualRepository + queries customizadas | ✅ |
| PrevisaoCicloRepository | ✅ |
| MotorPreditivoCiclo — media movel ponderada | ✅ |
| CicloService — orquestracao de regras | ✅ |
| CicloController — 5 endpoints REST | ✅ |
| GlobalExceptionHandler com erros especificos | ✅ |
| Migration V2 — tabelas ciclo_menstrual e previsao_ciclo | ✅ |
| CorsConfig para desenvolvimento local | ✅ |

---

## O que Funcionou Bem

- Motor preditivo implementado limpo, separando calculo de media, desvio padrao e nivel de confianca em metodos privados
- Arquitetura em camadas seguida corretamente na parte nova (application/domain/infrastructure)
- Uso de Records Java para DTOs — zero boilerplate
- Tratamento de erros com excecoes especificas (CicloNaoEncontradoException, CicloEmAbertoException)
- Logs estruturados com @Slf4j em todos os servicos

---

## O que Pode Melhorar

- Flyway ficou desabilitado — o schema esta sendo gerenciado por ddl-auto=update
- Credenciais hardcoded em application.properties (root/root)
- show-sql=true ativo (performance e seguranca)
- Package naming nao convencional (Cuidado.Feminino.API em uppercase)
- Frontend nao foi integrado com os novos endpoints

---

## Bloqueios que Apareceram

- Flyway desabilitado para simplificar o setup inicial — precisa ser resolvido antes do Sprint 04
- Arquivos do Sprint 00 (RegistroSaude, RegistroRepository, RegistroController) ainda fora da arquitetura correta

---

## Velocidade da Sprint

- Tarefas planejadas: 10
- Tarefas concluidas: 10
- Tarefas arrastadas: 0 (backend completo)

---

## Objetivo da Proxima Sprint

> Sprint 02: Implementar o calendario dinamico de ciclo menstrual no frontend com codigo de cores (vermelho/azul/amarelo) integrado com os endpoints do Sprint 01.

## Acoes para a Proxima Sprint

- [ ] Atualizar `api.ts` — trocar baseURL de `/api/saude` para `/api/v1`
- [ ] Decidir: substituir CalendarioPage atual ou criar nova aba "Ciclo"
- [ ] Implementar integracao com `GET /api/v1/ciclos/calendario?meses=3`
- [ ] Renderizar dias de menstruacao (vermelho), fertil (azul), ovulacao (amarelo)
- [ ] Criar formulario de inicio/encerramento de ciclo no frontend
- [ ] Exibir nivel de confianca da previsao para a usuaria
- [ ] Reativar Flyway e testar migrations do zero

## Adendo — 27/08/2026

O frontend de ciclo foi concluído com escopo replanejado para local-first, não por integração direta com `/api/v1/ciclos`. Flyway foi retomado no backend. O resultado e o backlog remanescente estão em [[Sprint 02 - Frontend Ciclo]].
