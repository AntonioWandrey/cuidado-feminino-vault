---
data: 2025-01-01
status: aceito
tags:
  - decisao
  - arquitetura
  - backend
---

# ADR-001 - Java 21 + Spring Boot

## Contexto

O projeto precisava de uma stack backend robusta, com suporte a longo prazo e boa integracao com JPA/Hibernate. O contexto academico da UNIFEBE tambem favorece Java por ser a linguagem principal do curso.

## Decisao

Usar Java 21 (LTS) com Spring Boot como framework principal do backend.

> **Versao real em uso:** Spring Boot **4.0.4** (gerado via Spring Initializr em 2026).
> O vault originalmente documentava "3.x", mas o projeto foi inicializado com a versao mais recente disponivel.

## Consequencias

**Pros:**
- Java 21 e LTS com suporte garantido ate 2029
- Spring Boot 4.x tem suporte nativo a Java 21 e Virtual Threads
- Ecossistema maduro: Spring Security, Spring Data JPA, Spring Validation
- Alinhado com o curriculo da UNIFEBE
- Grande comunidade e documentacao abundante

**Contras e Riscos:**
- Verbosidade maior que Node.js ou Python para APIs simples
- Spring Boot 4.x requer Jakarta EE 11 (jakarta.* em vez de javax.*)
- Curva de aprendizado inicial do ecossistema Spring

## Alternativas Consideradas

| Alternativa | Por que foi descartada |
|-------------|------------------------|
| Node.js + Express | Menos alinhado com o curso |
| Python + FastAPI | Fora do contexto academico |
| Quarkus | Menos maduro para o contexto do projeto |

## Referencias

- https://docs.spring.io/spring-boot/docs/current/reference/html/
- https://openjdk.org/projects/jdk/21/
