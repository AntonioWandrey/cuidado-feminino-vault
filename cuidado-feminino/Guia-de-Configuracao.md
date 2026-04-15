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

### Obrigatorios

| Plugin | Para que serve |
|--------|----------------|
| Templater | Templates com variaveis dinamicas (data, numero) |
| Dataview | Consultas nas notas - lista ADRs, referencias, etc |
| Obsidian Git | Sincroniza o vault com GitHub |

### Recomendados

| Plugin | Para que serve |
|--------|----------------|
| Calendar | Visualiza notas diarias no calendario |
| Advanced Tables | Editar tabelas Markdown facilmente |

---

## 4. Configurar Obsidian Git

1. Instala o plugin Obsidian Git
2. Cria um repositorio no GitHub: `cuidado-feminino-vault`
3. No terminal, dentro da pasta do vault:

```bash
git init
git remote add origin https://github.com/SeuUsuario/cuidado-feminino-vault.git
git add .
git commit -m "chore: setup inicial do vault"
git push -u origin main
```

4. Configuracoes do plugin:
   - Vault backup interval: 30 minutos
   - Auto pull on startup: ativado
   - Commit message: `vault: backup automatico`

---

## 5. Configurar Templater

1. Settings > Templater
2. Template folder location: `00-inbox/templates`
3. Trigger Templater on new file creation: ativado

**Como usar:**
- Cria uma nova nota
- Ctrl+P > digita "Templater: Insert Template"
- Escolhe o template desejado

---

## 6. Atalhos Uteis

| Atalho | Acao |
|--------|------|
| Ctrl+N | Nova nota |
| Ctrl+P | Command palette |
| Ctrl+O | Abrir nota (busca rapida) |
| Ctrl+E | Alternar edicao e preview |
| Ctrl+Shift+F | Busca em todo o vault |
| [[ | Criar link para outra nota |

---

## 7. Tags do Projeto

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
#sprint-01
#sprint-02
```
