---
data: 2026-08-28
prazo: 2026-08-28 23:59
tags:
  - entrega
  - sprint
  - calendario
  - artigos
---

# Entrega Semana 1 — status final em 28/08/2026

## Parecer

| Entrega | Estado |
|---|---|
| Gestão e integração de artigos | GO: demonstração Web local validada com MySQL |
| Calendário menstrual preliminar | GO: demonstração Web local validada e independente do backend |
| APK Android | Não validado nesta máquina; não usar como plano principal |

## Artigos

Entregue:

- CRUD administrativo em `/api/admin/conteudos`;
- editor Tiptap com texto formatado, links, imagem e YouTube;
- publicação/despublicação determinística e exclusão lógica;
- sanitização jsoup no backend e DOMPurify no frontend;
- filtros públicos de conteúdo inativo, inclusive por ID e séries;
- compatibilidade com texto legado e múltiplas fontes HTTPS;
- erro real quando a API está indisponível;
- 43 testes backend e 44 testes frontend;
- smoke criar → publicar → visualizar → editar → sanitizar → excluir.

Limite: a gestão não possui autenticação e só pode ser usada localmente.

## Calendário

Entregue:

- início e fim do período;
- armazenamento local versionado;
- visualização e identificação no calendário;
- histórico expansível;
- previsão baseada em intervalos entre inícios;
- persistência comprovada após recarga;
- roteiro de vídeo inferior a 6 minutos.

Limites: sem criptografia, backup, sincronização ou APK validado nesta máquina.

## Evidências

- repositório: `ANALISE_ENTREGAS_28-08-2026.md`;
- roteiro de artigos: `docs/entregas/semana-1-entrega-1-artigos.md`;
- roteiro de calendário: `ENTREGA_SEMANA1_CALENDARIO.md`;
- inicialização: `INICIAR_PROJETO_VSCODE.md`;
- decisões: [[ADR-005 - Calendario local-first e previsao no dispositivo]] e [[ADR-007 - Conteudo rico HTML sanitizado com Tiptap]].
