# Cuidado Feminino - Vault

> Cerebro tecnico do projeto. Decisoes, padroes, estudos e base do TCC.

---

## Mapa do Vault

| Pasta | Para que serve |
|-------|----------------|
| 00-inbox | Ideias soltas, anotacoes rapidas sem classificacao |
| 01-projeto | ADRs, reunioes, retrospectivas de sprint |
| 02-tecnico | Padroes de codigo, banco, backend, frontend |
| 03-tcc | Referencias academicas, rascunhos, reunioes com orientador |
| 04-git-journal | Log de commits e decisoes tecnicas importantes |

---

## Acesso Rapido

- [[ADR-001 - Java 21 + Spring Boot 3]]
- [[ADR-002 - MariaDB como banco principal]]
- [[ADR-003 - Media movel ponderada para ciclo]]
- [[Padroes - Conventional Commits]]
- [[Padroes - Clean Code e SOLID]]
- [[Banco - Modelagem de Ciclo Menstrual]]
- [[Backend - Arquitetura Spring Boot]]
- [[Frontend - Arquitetura React]]
- [[TCC - Visao Geral e Cronograma]]

---

## Status do Projeto

| Sprint | Foco | Status |
|--------|------|--------|
| Sprint 00 | Setup fullstack + persistencia de sintomas | ✅ Concluido |
| Sprint 01 | Motor Preditivo de Ciclo Menstrual | ✅ Concluido |
| Sprint 02 | Calendario dinamico com codigo de cores | 🔄 Ativo |
| Sprint 03 | Log sexual, contraceptivos, libido | ⏳ Futuro |
| Sprint 04 | JWT Stateless + relatorios medicos PDF | ⏳ Futuro |

- **Repositorio:** https://github.com/AntonioWandrey/cuidado-feminino-fullstack
- **Notion:** (cole o link aqui)

---

## Dividas Tecnicas Conhecidas

- [ ] Flyway desabilitado em `application.properties` — reativar apos resolver compatibilidade
- [ ] `spring.jpa.hibernate.ddl-auto=update` na dev — trocar para `validate` quando Flyway estiver ativo
- [ ] `api.ts` no frontend ainda aponta para `/api/saude` (Sprint 00) — atualizar para `/api/v1/ciclos`
- [ ] Arquivos fora da arquitetura: `model/RegistroSaude.java`, `repository/RegistroRepository.java`, `controller/RegistroController.java` precisam ser movidos ou removidos
- [ ] Package naming `Cuidado.Feminino.API` (uppercase) — nao convencional em Java
- [ ] Credenciais hardcoded em `application.properties` — mover para variaveis de ambiente
- [ ] `spring.jpa.show-sql=true` — desabilitar antes de producao

---

## Links Uteis

- [GitHub](https://github.com/AntonioWandrey/cuidado-feminino-fullstack)
- [Spring Boot Docs](https://docs.spring.io/spring-boot/docs/current/reference/html/)
- [React Docs](https://react.dev)
- [MariaDB Docs](https://mariadb.com/kb/en/)
- [LGPD - Lei 13.709/2018](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm)
