---
tags:
  - backend
  - arquitetura
  - spring
  - java
atualizado: 2026-08-28
---

# Backend — arquitetura Spring Boot

## Stack

| Tecnologia | Estado |
|---|---|
| Java | 21 LTS |
| Spring Boot | 4.0.4 |
| Hibernate ORM | 7.2.x |
| Spring Data JPA | 8 repositórios detectados |
| Flyway | migrations V1–V4 |
| MySQL Connector/J | banco de desenvolvimento |
| Maven Surefire | 3.5.6 |
| jsoup | sanitização de HTML rico |
| H2 | banco isolado da suíte de testes |

## Módulos implementados

```text
Cuidado.Feminino.API
├── ciclo menstrual e previsão remota
├── conteúdo educativo público e administrativo
├── sanitização de HTML e mídia
├── categorias e séries
├── registro de queixas
├── tratamento global de erros
├── CORS e Actuator
└── DataSeeder
```

O package raiz em maiúsculas é legado e não segue a convenção Java; a correção exige refatoração coordenada.

## Endpoints principais

### Ciclos remotos

- `POST /api/v1/ciclos`;
- `PATCH /api/v1/ciclos/{id}/encerrar`;
- `GET /api/v1/ciclos`;
- `GET /api/v1/ciclos/previsao`;
- `GET /api/v1/ciclos/calendario`.

Esses endpoints não são consumidos pelo calendário local da Entrega 2.

### Conteúdo e queixas

- `/api/conteudos`, `/api/categorias`, `/api/series` para leitura;
- `/api/admin/conteudos` para criar, editar, publicar e excluir logicamente;
- `/api/queixas` para registro e consulta.

O controller administrativo é exclusivo da demonstração local enquanto não houver autenticação. Consultas públicas e séries filtram conteúdos inativos. O HTML é sanitizado antes da persistência; links e mídia usam HTTPS, SVG/SVGZ são bloqueados e YouTube é normalizado sem autoplay.

## Configuração local

O arquivo rastreado ativa os perfis `local,dev`. Credenciais ficam em `application-local.properties`, criado a partir do exemplo e ignorado pelo Git.

```powershell
cd backend\cuidado-feminino-api
Copy-Item src\main\resources\application-local.properties.example src\main\resources\application-local.properties
.\mvnw.cmd spring-boot:run
```

## Testes e riscos

- 43 testes aprovados em 28/08/2026;
- contexto usa perfil `test` e H2 em memória, sem tocar no MySQL local;
- smoke CRUD aprovado com MySQL e artigo de demonstração;
- migrations MySQL não são executadas pelo perfil H2 e ainda exigem validação própria;
- `usuarioId = 1` ainda aparece no protótipo remoto;
- o motor remoto confunde duração do sangramento com intervalo do ciclo;
- endpoints administrativos exigem autenticação antes de publicação externa.
