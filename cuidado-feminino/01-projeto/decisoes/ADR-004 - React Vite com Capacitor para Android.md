---
data: 2026-08-27
status: aceito
tags:
  - decisao
  - arquitetura
  - frontend
  - android
---

# ADR-004 — React/Vite com Capacitor para Android

## Contexto

O cliente existente já usa React, TypeScript e Vite. A equipe precisa executar a mesma interface no navegador e, futuramente, distribuir um aplicativo Android sem reescrever toda a UI em Kotlin.

## Decisão

Manter React/Vite como cliente Web e usar Capacitor 8 para gerar e sincronizar o projeto Android. O diretório Web produzido é `frontend/dist`, copiado para o projeto nativo por `npx cap sync android`.

O diretório `frontend/android` é versionado; caches, builds, `local.properties` e assets copiados permanecem ignorados.

## Consequências

### Positivas

- uma base de interface para Web e Android;
- acesso gradual a APIs nativas;
- demonstração Web independente do SDK Android;
- menor custo que uma reescrita nativa.

### Negativas e riscos

- build Android depende de Android Studio, SDK e Gradle compatíveis;
- plugins Capacitor precisam ser sincronizados após mudanças;
- URLs locais de API diferem entre navegador, emulador e dispositivo físico;
- não se deve editar arquivos declarados como gerados pelo Capacitor.

## Procedimento

```powershell
cd frontend
npm run build
npx cap sync android
npx cap open android
```

## Alternativas consideradas

| Alternativa | Avaliação |
|---|---|
| PWA apenas | Mais simples, porém não atende todas as necessidades de distribuição nativa |
| Kotlin nativo | Melhor integração nativa, com custo alto de reimplementação |
