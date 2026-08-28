---
tags:
  - frontend
  - arquitetura
  - react
  - typescript
atualizado: 2026-08-28
---

# Frontend — arquitetura React

## Stack atual

| Tecnologia | Versão/função |
|---|---|
| React | 18.3.1 |
| TypeScript | 5.8.3, strict mode |
| Vite | 8.0.0 |
| Tailwind CSS | 3.4.17 |
| TanStack Query | 5.83.0 |
| Axios | 1.13.6 |
| Vitest | 4.1.0 |
| Capacitor | 8.3.4 |
| Tiptap | 3.30.5, editor de artigos |
| DOMPurify | 3.4.14, sanitização defensiva |

## Fluxos de dados

```text
CalendarioPage
└── cicloService
    ├── valida início, fim e sobreposição
    ├── localStorage: cuidado-feminino:ciclos:v1
    └── calcula previsão local

Conteúdos públicos / gestão / categorias / séries / queixas
└── services → api.ts → Spring Boot
    ├── conteúdo real sem fallback fictício
    └── refetch em montagem, foco e intervalo de demonstração

Build Vite
└── dist → Capacitor sync → android/app/src/main/assets/public
```

## Páginas

- `HomePage`: resumo, atalhos e conteúdo em destaque;
- `CalendarioPage`: calendário menstrual, período real, previsão, histórico e queixas;
- `ConteudosPage`: listagem, busca e filtros;
- `ConteudoDetalhePage`: leitura do conteúdo;
- `GestaoArtigosPage`: listagem administrativa, publicação e exclusão;
- `ArtigoFormPage`: criação, edição, Tiptap e prévia segura;
- `TrilhasPage`: séries e trilhas;
- `PerfilPage`: estatísticas e regularidade;
- `NotFound`: rota inválida.

## Decisões importantes

- O serviço de ciclo não passa pelo Axios nem pelo backend.
- Datas do ciclo usam `yyyy-MM-dd` e formatação local para evitar deslocamento por fuso.
- React Query orquestra atualização de tela mesmo para operações locais.
- Capacitor reaproveita o build Web; Android não é uma segunda base de UI.
- O conteúdo rico é sanitizado defensivamente e texto legado é normalizado antes da leitura/edição.

## Limitações

- `localStorage` não criptografa dados;
- categorias/séries/queixas ainda possuem fallback de contingência; conteúdo público e gestão exibem erros reais;
- não há autenticação nem sincronização de ciclos;
- a gestão Web não pode ser publicada externamente sem autenticação;
- o bundle principal ainda gera aviso de tamanho acima de 500 kB.

## Verificação

Em 28/08/2026: 44 testes aprovados em 12 arquivos, typecheck e build Vite aprovados, ESLint com 0 erros e 3 avisos preexistentes de Fast Refresh. Smoke visual de artigos e calendário aprovado no navegador.
