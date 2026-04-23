---
data: 2026-04-17
tags:
  - git
  - dev
  - infra
  - flyway
  - java-21
---

# Git Journal - 2026-04-17 (Pos-upgrade Java 21 + Flyway ativado)

> Commits realizados em 2026-04-15, documentados em 2026-04-17.
> Encerram as dividas tecnicas de infraestrutura abertas ao fim do Sprint 01.

---

## Commits

| Hash | Data | Descricao | Layer |
|------|------|-----------|-------|
| `c442b8f` | 2026-04-15 | chore(infra): finaliza pos-upgrade Java 21 e habilita Flyway | backend/infra |
| `f5d5f0f` | 2026-04-15 | chore(infra): atualiza configuracao do VS Code para Java | ide/infra |

---

## c442b8f — Finaliza pos-upgrade Java 21 e habilita Flyway

### O que mudou

**pom.xml**
- `java.version` corrigido de `25` para `21` (script de upgrade havia inserido valor incorreto)
- Adicionado `maven.compiler.release=21` — garante que o bytecode gerado seja compativel com Java 21
- Substituidas 3 dependencias de teste inexistentes (`spring-boot-starter-data-jpa-test`, `spring-boot-starter-validation-test`, `spring-boot-starter-webmvc-test`) por `spring-boot-starter-test` (unica dependencia correta de teste no Spring Boot)

**application.properties**
- `spring.jpa.hibernate.ddl-auto` mudado de `update` para `validate` — Flyway agora gerencia o schema, Hibernate apenas valida
- `spring.flyway.enabled=true` — Flyway reativado (concern de compatibilidade com Spring Boot 4.x foi resolvido)
- `spring.flyway.baseline-on-migrate=true` — suporta banco ja existente sem recriar do zero
- `spring.flyway.baseline-version=0` — define a versao de baseline

**CorsConfig.java**
- CORS expandido de apenas `http://localhost:5173` para 4 portas (`5173`, `5174`, `5175`, `5176`)
- Motivo: Vite pode incrementar a porta automaticamente se 5173 estiver ocupada

**frontend/src/components/ui/sonner.tsx**
- Removida dependencia `next-themes` do componente Toaster (nao e utilizada no projeto)

**.mvn/wrapper/maven-wrapper.properties**
- Maven Wrapper downgraded de `4.0.0-rc-5` para `3.9.12`
- Motivo: `4.0.0-rc-5` e uma release candidate instavel; `3.9.12` e a versao estavel atual do Maven 3.x, compativel com Spring Boot 4.x e Java 21

### Impacto

| Divida Tecnica (Sprint 01) | Status Antes | Status Depois |
|---------------------------|--------------|---------------|
| Flyway desabilitado | ❌ DIVIDA | ✅ Resolvida |
| `ddl-auto=update` em uso | ❌ DIVIDA | ✅ Trocado para `validate` |
| Dependencias de teste invalidas no pom.xml | ❌ Bug latente | ✅ Corrigido |
| `java.version=25` no pom.xml | ❌ Bug latente | ✅ Corrigido para 21 |
| CORS limitado a porta 5173 | ⚠️ Fragil | ✅ Expandido (5173–5176) |

---

## f5d5f0f — Atualiza configuracao do VS Code para Java

### O que mudou

**.vscode/settings.json**
- Adicionado `java.configuration.updateBuildConfiguration: "interactive"` — impede que o VS Code sincronize automaticamente o `pom.xml` sem confirmacao do desenvolvedor

### Por que importa

Apos alteracoes no pom.xml (como as do commit anterior), o VS Code/Extension Pack for Java tentava reindexar o projeto automaticamente, podendo causar interferencias no build. O modo `interactive` exige confirmacao manual.

---

## Estado das Dividas Tecnicas (Pos Commits)

> Referencia: lista aberta ao final do Sprint 01

| Divida | Status |
|--------|--------|
| ~~Flyway desabilitado~~ | ✅ Resolvida |
| ~~ddl-auto=update~~ | ✅ Resolvida |
| Credenciais hardcoded (`root/root`) | ❌ Ainda aberta |
| `show-sql=true` ativo | ❌ Ainda aberta |
| `api.ts` no frontend aponta para `/api/saude` (Sprint 00) | ❌ Ainda aberta — Sprint 02 |

---

## Estrategia de Branches (Adotada neste periodo)

> A partir do upgrade Java 21, o projeto passou a usar um fluxo com `develop` separado do `main`.

| Branch | Papel |
|--------|-------|
| `main` | Codigo estavel — Sprint 01 concluido (`9d154b2`) |
| `develop` | Branch ativa — recebe feat, fix, chore antes de ir para main |
| `appmod/java-upgrade-20260409154222` | Gerada automaticamente pela ferramenta de upgrade — residual |

Os 4 commits deste journal vivem em `develop` e ainda **nao foram mergeados** no `main`.
Os commits "Step 1" e "Step 3" sao automaticos da ferramenta AppMod — nao seguem Conventional Commits intencionalmente.

Ver: [[Padroes - Estrategia Git]]

---

## Aprendizados

- `maven.compiler.release` e diferente de `java.version` no pom.xml — o primeiro controla o bytecode gerado, o segundo e usado por plugins do Spring Boot. Ambos precisam estar alinhados.
- `baseline-on-migrate=true` no Flyway e essencial quando o banco ja existe antes de o Flyway ser ativado — sem isso, o Flyway rejeita o banco por nao ter o historico de migrations.
- Flyway com Spring Boot 4.x funciona normalmente — o concern inicial era infundado.
- Dependencias como `spring-boot-starter-data-jpa-test` nao existem no ecossistema Spring — o correto e sempre `spring-boot-starter-test`.
