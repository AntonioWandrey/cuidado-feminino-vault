---
tags:
  - padroes
  - git
  - branching
  - workflow
criado_em: 2026-04-17
---

# Padroes - Estrategia Git (Branching)

> Workflow de branches adotado no projeto Cuidado Feminino a partir do Sprint 02.
> Contexto: definido apos o upgrade Java 21, que introduziu a branch `develop`.

---

## Estrutura de Branches

```
main
 └── develop  ← branch ativa de desenvolvimento
       ├── feat/...    (funcionalidades novas)
       ├── fix/...     (correcoes)
       └── chore/...   (manutencao, infra)
```

| Branch | Papel | Quem commita |
|--------|-------|--------------|
| `main` | Producao / codigo estavel | Apenas via merge do `develop` |
| `develop` | Integracao continua de desenvolvimento | Commits diretos (feat, fix, chore) e merges de feature branches |
| `appmod/*` | Branches automaticas geradas por ferramentas externas (ex: AppMod Java Upgrade) | Ferramenta — nao editar manualmente |

---

## Regra de Ouro

> **Nunca commitar diretamente na `main`.**
> Todo desenvolvimento passa pelo `develop` primeiro.

---

## Fluxo de Trabalho

```
1. Trabalho acontece em develop (ou branch de feature a partir dela)
2. feat/fix/chore sao commitados em develop
3. Quando develop esta estavel e testado → merge em main
4. main representa sempre um estado deployavel
```

---

## Estado Atual das Branches (2026-04-17)

| Branch | HEAD | Status |
|--------|------|--------|
| `main` | `9d154b2` — chore(frontend): remove artefatos do Lovable | Estavel — Sprint 01 concluido |
| `develop` | `f5d5f0f` — chore(infra): atualiza config VS Code para Java | 4 commits a frente do main — aguardando merge |
| `appmod/java-upgrade-20260409154222` | `f5d5f0f` | Branch residual do upgrade automatico — nao sera mergeada diretamente |

### Commits exclusivos do `develop` (ainda nao em `main`)

| Hash | Descricao |
|------|-----------|
| `d870dc3` | Step 1: Setup Environment - Environment setup complete |
| `caf6a9b` | Step 3: Upgrade Java Version - Compile: SUCCESS |
| `c442b8f` | chore(infra): finaliza pos-upgrade Java 21 e habilita Flyway |
| `f5d5f0f` | chore(infra): atualiza configuracao do VS Code para Java |

> Os commits `Step 1` e `Step 3` foram gerados automaticamente pela ferramenta AppMod durante o upgrade do Java. Nao seguem Conventional Commits — comportamento esperado para commits de ferramentas externas.

---

## Branch `appmod/java-upgrade-20260409154222`

Branch criada automaticamente pela ferramenta **Amazon AppMod** (ou equivalente) para executar o upgrade de Java em etapas rastreadas ("Step 1", "Step 3"). Aponta para o mesmo HEAD do `develop`.

- Nao deve ser editada manualmente
- Pode ser deletada apos o merge do `develop` na `main`
- O sufixo `20260409154222` e um timestamp gerado pela ferramenta

---

## Proximos Passos

- [ ] Merge do `develop` na `main` apos validacao do Sprint 02
- [ ] Deletar branch `appmod/java-upgrade-20260409154222` apos merge
