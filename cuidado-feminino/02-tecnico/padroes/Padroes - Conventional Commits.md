---
tags:
  - padroes
  - git
  - convencao
---

# Padroes - Conventional Commits

> Convencao de mensagens de commit adotada no projeto Cuidado Feminino.

---

## Formato

```
<tipo>(escopo): <descricao curta>

[corpo opcional]
```

## Tipos

| Tipo | Quando usar | Exemplo |
|------|-------------|---------|
| feat | Nova funcionalidade | feat(ciclo): adicionar endpoint de previsao |
| fix | Correcao de bug | fix(auth): corrigir validacao de token expirado |
| refactor | Refatoracao sem mudanca de comportamento | refactor(service): extrair logica de media para metodo privado |
| docs | Documentacao | docs(readme): atualizar instrucoes de setup |
| test | Adicionar ou corrigir testes | test(ciclo): adicionar teste unitario do motor preditivo |
| chore | Tarefas de manutencao | chore(deps): atualizar spring-boot para 3.2.1 |
| style | Formatacao sem mudanca de logica | style(controller): aplicar formatacao do checkstyle |
| perf | Melhoria de performance | perf(query): adicionar indice em ciclo_menstrual |
| ci | Pipeline e CI/CD | ci(github): adicionar workflow de build |

## Escopos do Projeto

| Escopo | Representa |
|--------|------------|
| ciclo | Motor preditivo e endpoints de ciclo |
| sintoma | Registro de sintomas |
| auth | Autenticacao JWT |
| calendario | Componente de calendario React |
| banco | Migrations e modelagem |
| relatorio | Geracao de PDF clinico |
| infra | Configuracoes, properties, Docker |

## Exemplos Reais do Projeto

```bash
feat(banco): criar migration V2 com tabelas ciclo_menstrual e previsao_ciclo
feat(ciclo): implementar entidade CicloMenstrual com calculo automatico de duracao
feat(ciclo): criar MotorPreditivoCiclo com media movel ponderada
feat(ciclo): adicionar endpoint POST /ciclos para registrar inicio de ciclo
feat(calendario): criar componente CalendarView com renderizacao por cores
fix(ciclo): corrigir calculo de dias quando data_fim e null
refactor(service): separar logica de confianca em metodo privado
docs(api): documentar endpoints de ciclo no Notion
test(motor): adicionar testes unitarios para calculo de ovulacao
```

## Regras

1. Descricao em portugues e letra minuscula
2. Sem ponto final na descricao curta
3. Maximo de 72 caracteres na linha de descricao
4. Use o corpo para explicar o PORQUE, nao o QUE
