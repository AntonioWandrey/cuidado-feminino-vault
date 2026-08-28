---
data: 2026-08-28
status: aceito
tags:
  - decisao
  - arquitetura
  - conteudo
  - seguranca
  - frontend
  - backend
---

# ADR-007 — Conteúdo rico HTML sanitizado com Tiptap

## Contexto

A Entrega 1 exige criar e editar artigos com texto formatado, imagens e vídeos usando um editor pronto. O modelo já possui o campo `corpo` do tipo `TEXT`, e não há tempo ou necessidade de introduzir armazenamento binário de mídia.

## Decisão

- usar Tiptap como Rich Text Editor no frontend;
- armazenar HTML sanitizado no campo `corpo` existente;
- aceitar imagens somente por URL HTTPS;
- aceitar vídeos somente do YouTube, normalizados para `youtube-nocookie.com` sem query, fragmento ou autoplay;
- sanitizar no backend antes de persistir;
- sanitizar novamente no frontend antes de renderizar;
- manter controller público somente leitura e mutações em `/api/admin/conteudos`;
- limitar os endpoints administrativos ao ambiente local enquanto não houver autenticação.

## Lista permitida

Elementos editoriais básicos: parágrafos, H2/H3, ênfase, sublinhado, listas, citação, links HTTPS, imagens HTTPS e vídeo do YouTube. Scripts, eventos `on*`, formulários, objetos, SVG/SVGZ, `javascript:`, `data:`, autoplay e iframes arbitrários são removidos ou rejeitados. O campo de referências aceita uma ou mais URLs HTTPS separadas por `|`, preservando compatibilidade com o seed existente.

## Evidência de implementação

Em 28/08/2026, o fluxo criar → publicar → visualizar → editar → excluir foi validado com MySQL. Um payload contendo script, eventos, SVGZ e `autoplay=1` foi sanitizado no backend e renderizado defensivamente no frontend. A suíte final registrou 43 testes backend e 44 frontend.

## Consequências

### Positivas

- atende ao fluxo acadêmico sem migration adicional;
- preserva formatação entre gestão Web e aplicativo;
- reduz risco de XSS com defesa em profundidade;
- evita payloads Base64 e biblioteca de mídia prematura.

### Negativas e riscos

- mídia externa pode ficar indisponível;
- sanitizadores do frontend e backend precisam permanecer alinhados;
- HTML existente pode perder marcação não permitida;
- endpoints administrativos não são seguros para exposição pública sem autenticação e autorização.

## Alternativas consideradas

| Alternativa | Motivo para não escolher |
|---|---|
| Markdown | Não atende tão diretamente ao editor visual e embeds exigidos |
| Upload binário/CDN | Amplia infraestrutura, segurança e custo fora do escopo |
| Iframe/HTML livre | Risco inaceitável de XSS e conteúdo arbitrário |
