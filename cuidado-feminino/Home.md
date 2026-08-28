# Cuidado Feminino — Vault

> Fonte de decisões arquiteturais, status acadêmico e referências técnicas do projeto Minha Saúde Feminina.

Atualizado em: 28/08/2026.

## Acesso rápido

- [[Entrega Semana 1 - Status 2026-08-28]]
- [[ADR-001 - Java 21 + Spring Boot 4]]
- [[ADR-002 - MySQL compativel com MariaDB]]
- [[ADR-003 - Motor preditivo remoto por media ponderada]]
- [[ADR-004 - React Vite com Capacitor para Android]]
- [[ADR-005 - Calendario local-first e previsao no dispositivo]]
- [[ADR-006 - Sincronizacao futura com Spring e banco relacional]]
- [[ADR-007 - Conteudo rico HTML sanitizado com Tiptap]]
- [[Frontend - Arquitetura React]]
- [[Backend - Arquitetura Spring Boot]]
- [[Banco - Modelagem de Ciclo Menstrual]]
- [[Sprint 02 - Frontend Ciclo]]
- [[Padroes - Conventional Commits]]
- [[Padroes - Clean Code e SOLID]]

## Estado do projeto

| Área | Estado real |
|---|---|
| Calendário menstrual local-first | Pronto para demonstração Web em 28/08/2026 |
| Gestão e integração de artigos | Pronta para demonstração Web local em 28/08/2026 |
| Conteúdo público | CRUD, Tiptap, sanitização, filtros e integração validados |
| Registro de queixas | Implementado no backend e frontend |
| Backend Spring/MySQL | 43 testes isolados em H2; smoke real aprovado com MySQL |
| Capacitor/Android | Projeto sincronizado; APK não validado por falta de SDK local |
| Autenticação e LGPD de produção | Backlog |

## Arquitetura em uma frase

O calendário usa React → `cicloService` → `localStorage`; conteúdo e queixas usam React → Axios → Spring Boot → MySQL; a sincronização futura dos ciclos permanece proposta no [[ADR-006 - Sincronizacao futura com Spring e banco relacional]].

## Dívidas prioritárias

- [x] Implementar a Entrega 1 de artigos com CRUD administrativo e editor rico.
- [ ] Corrigir no backend a diferença entre duração do sangramento e duração do ciclo.
- [ ] Definir autenticação, consentimento e política de sincronização.
- [ ] Criptografar ou substituir o armazenamento local antes de uso real.
- [x] Adicionar testes de serviço/controller no backend.
- [ ] Proteger `/api/admin/**` com autenticação e autorização antes de deploy externo.
- [ ] Dividir o bundle Web principal para reduzir o aviso acima de 500 kB.
- [ ] Preparar Android SDK e validar um APK atual.

## Repositórios

- Projeto: https://github.com/AntonioWandrey/cuidado-feminino-fullstack
- Vault: https://github.com/AntonioWandrey/cuidado-feminino-vault
