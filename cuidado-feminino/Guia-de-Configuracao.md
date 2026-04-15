# Guia de Configuracao do Obsidian

> Siga este guia apos copiar os arquivos para o vault.
> Acesse as configuracoes em: Obsidian > icone de engrenagem

---

## 1. Files and Links

- **Default location for new notes:** 00-inbox
- **Use Wikilinks:** ativado
- **Detect all file extensions:** ativado

---

## 2. Core Plugins - Ativar

| Plugin | Por que |
|--------|---------|
| Templates | Usar os templates da pasta 00-inbox/templates |
| Daily Notes | Criar nota diaria rapidamente |
| Backlinks | Ver onde uma nota e citada |
| Outline | Navegar pelos titulos da nota |
| Tag Pane | Ver todas as tags do vault |
| Search | Busca full-text |

**Configurar Templates:**
1. Settings > Core Plugins > Templates
2. Template folder location: `00-inbox/templates`

---

## 3. Community Plugins - Instalar

Settings > Community Plugins > Browse

### Obrigatorios (ja instalados)

| Plugin | ID | Para que serve |
|--------|-----|----------------|
| Templater | templater-obsidian | Templates com variaveis dinamicas (data, numero) |
| Obsidian Git | obsidian-git | Sincroniza o vault com GitHub |
| Calendar | calendar | Visualiza notas diarias no calendario |

### Obrigatorios (instalar agora)

| Plugin | ID | Para que serve |
|--------|-----|----------------|
| Dataview | dataview | Consultas nas notas — lista ADRs por status, referencias por capitulo, etc. |
| Advanced Tables | advanced-tables | Editar tabelas Markdown facilmente com Tab e Enter |

**Como instalar:**
1. Settings > Community Plugins > Browse
2. Pesquisar pelo nome
3. Install > Enable

### Recomendados

| Plugin | ID | Para que serve |
|--------|-----|----------------|
| Kanban | obsidian-kanban | Board de tarefas por sprint dentro do vault |
| Excalidraw | obsidian-excalidraw-plugin | Diagramas e sketches de arquitetura |

---

## 4. Configurar Obsidian Git

Plugin ja instalado. Configuracoes:

1. Settings > Obsidian Git
   - Vault backup interval (minutos): `30`
   - Auto pull on startup: `ativado`
   - Commit message: `vault: backup automatico`
   - Pull updates on startup: `ativado`

2. Repositorio configurado: `cuidado-feminino-vault` no GitHub

---

## 5. Configurar Templater

1. Settings > Templater
2. Template folder location: `00-inbox/templates`
3. Trigger Templater on new file creation: `ativado`

**Templates disponiveis:**
- `Template-ADR.md` — para novas decisoes arquiteturais
- `Template-Git-Journal.md` — para log diario de desenvolvimento
- `Template-Referencia-TCC.md` — para referencias bibliograficas
- `Template-Retrospectiva-Sprint.md` — para retrospectivas de sprint
- `Template-Reuniao.md` — para atas de reuniao

**Como usar:**
- Cria uma nova nota
- Ctrl+P > digita "Templater: Insert Template"
- Escolhe o template desejado

---

## 6. Configurar Dataview (apos instalar)

O Dataview permite consultas tipo SQL nas notas. Exemplos de uso para este vault:

**Listar todas as ADRs:**
~~~
```dataview
TABLE data, status
FROM "01-projeto/decisoes"
WHERE contains(tags, "decisao")
SORT data DESC
```
~~~

**Listar referencias por capitulo do TCC:**
~~~
```dataview
TABLE tipo, file.name
FROM "03-tcc"
WHERE contains(tags, "referencia")
```
~~~

---

## 7. Atalhos Uteis

| Atalho | Acao |
|--------|------|
| Ctrl+N | Nova nota |
| Ctrl+P | Command palette |
| Ctrl+O | Abrir nota (busca rapida) |
| Ctrl+E | Alternar edicao e preview |
| Ctrl+Shift+F | Busca em todo o vault |
| [[ | Criar link para outra nota |
| Ctrl+G | Abrir graph view |

---

## 8. Tags do Projeto

```
#decisao
#arquitetura
#banco
#backend
#frontend
#padroes
#reuniao
#tcc
#referencia
#git
#sprint-00
#sprint-01
#sprint-02
#sprint-03
#sprint-04
```
