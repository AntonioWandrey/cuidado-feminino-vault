---
tags:
  - backend
  - arquitetura
  - spring
  - java
---

# Backend - Arquitetura Spring Boot

> Guia de referencia da arquitetura adotada no projeto.
> Atualizado em: 2026-04-15

---

## Stack Real

| Tecnologia | Versao | Observacao |
|------------|--------|------------|
| Java | 21 (LTS) | |
| Spring Boot | 4.0.4 | Gerado via Initializr |
| Spring Data JPA | incluso no Boot | Hibernate como provider |
| Spring Validation | incluso no Boot | Bean Validation |
| Flyway | flyway-mysql | **DESABILITADO** — divida tecnica |
| Lombok | incluso | @Getter, @Setter, @Builder, @Slf4j |
| MySQL Driver | mysql-connector-j | Compativel com MariaDB |

---

## Estrutura de Pacotes Real

> **Atencao:** O package raiz usa uppercase (`Cuidado.Feminino.API`), o que nao e convencional em Java.
> O padrao correto seria `br.com.unifebe.saude` (conforme groupId no pom.xml).
> Manter o atual por ora para nao quebrar o build.

```
Cuidado.Feminino.API
├── application
│   ├── dto
│   │   ├── CicloRequest.java
│   │   ├── CicloResponse.java
│   │   ├── EncerrarCicloRequest.java
│   │   ├── PrevisaoResponse.java
│   │   └── CalendarioResponse.java
│   └── exception
│       ├── CicloNaoEncontradoException.java
│       ├── CicloEmAbertoException.java
│       └── GlobalExceptionHandler.java
├── domain
│   ├── entity
│   │   ├── CicloMenstrual.java        (com @PrePersist/@PreUpdate)
│   │   └── PrevisaoCiclo.java
│   ├── repository
│   │   ├── CicloMenstrualRepository.java
│   │   └── PrevisaoCicloRepository.java
│   └── service
│       ├── CicloService.java          (orquestra regras de negocio)
│       └── MotorPreditivoCiclo.java   (algoritmo preditivo — @Component)
└── infrastructure
    ├── controller
    │   └── CicloController.java       (/api/v1/ciclos)
    └── config
        └── CorsConfig.java

--- LEGADO (Sprint 00 — fora da arquitetura correta) ---
model/RegistroSaude.java           → mover para domain/entity/
repository/RegistroRepository.java → mover para domain/repository/
controller/RegistroController.java → mover para infrastructure/controller/
```

---

## Endpoints Implementados (Sprint 01)

| Metodo | Endpoint | Descricao | Status HTTP |
|--------|----------|-----------|-------------|
| POST | /api/v1/ciclos | Registrar inicio de ciclo | 201 Created |
| PATCH | /api/v1/ciclos/{id}/encerrar | Encerrar ciclo em aberto | 200 OK |
| GET | /api/v1/ciclos | Listar ciclos da usuaria | 200 OK |
| GET | /api/v1/ciclos/previsao | Gerar e obter previsao | 200 OK |
| GET | /api/v1/ciclos/calendario?meses=3 | Projecao para o calendario | 200 OK |

---

## Fluxo de uma Requisicao

```
React (Axios)
    POST /api/v1/ciclos
    CicloController.registrarInicio(@RequestBody @Valid CicloRequest)
    CicloService.registrarInicio(request)
        CicloMenstrualRepository.save(entity)
    return CicloResponse.de(entity)
    HTTP 201 Created

React (Axios)
    GET /api/v1/ciclos/previsao
    CicloController.obterPrevisao()
    CicloService.gerarPrevisao()
        CicloMenstrualRepository.findUltimos12CiclosCompletos(usuarioId)
        MotorPreditivoCiclo.calcularPrevisao(usuarioId, ultimaDataInicio, ciclos)
        PrevisaoCicloRepository.save(previsao)
    return PrevisaoResponse.de(previsao)
    HTTP 200 OK
```

---

## application.properties (Estado Real)

```properties
# Conexao MySQL
spring.datasource.url=jdbc:mysql://localhost:3306/cuidado_feminino?createDatabaseIfNotExist=true&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=root            # DIVIDA: mover para variavel de ambiente

# Hibernate
spring.jpa.hibernate.ddl-auto=update       # DIVIDA: trocar para validate quando Flyway ativo
spring.jpa.show-sql=true                   # DIVIDA: desabilitar antes de producao
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect
spring.jpa.open-in-view=false

# Flyway DESABILITADO
spring.flyway.enabled=false                # DIVIDA: reativar

# DevTools
spring.devtools.restart.enabled=false
```

**Estado ideal para producao:**
```properties
spring.datasource.username=${DB_USER:root}
spring.datasource.password=${DB_PASS:}
spring.jpa.hibernate.ddl-auto=validate
spring.jpa.show-sql=false
spring.flyway.enabled=true
spring.flyway.locations=classpath:db/migration
```

---

## Anotacoes Importantes

| Anotacao | Onde usar | Para que serve |
|----------|-----------|----------------|
| @RestController | Controller | Combina @Controller + @ResponseBody |
| @RequestMapping | Controller | Define a rota base |
| @Service | Service | Marca como bean de servico |
| @Component | Motor Preditivo | Bean generico gerenciado pelo Spring |
| @Repository | Repository | Marca como repositorio e traduz excecoes JPA |
| @Entity | Entity | Mapeia para tabela do banco |
| @PrePersist/@PreUpdate | Entity | Hooks do ciclo de vida JPA |
| @Valid | Controller params | Dispara validacao do Bean Validation |
| @Slf4j | Service/Component | Injeta logger via Lombok |
| @RequiredArgsConstructor | Service/Controller | Injecao de dependencia via construtor |
| @Transactional | Service methods | Controle de transacao |

---

## Tratamento Global de Erros

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(CicloNaoEncontradoException.class)
    public ResponseEntity<ErroResponse> handleNotFound(CicloNaoEncontradoException ex) {
        return ResponseEntity.status(404).body(new ErroResponse(ex.getMessage()));
    }

    @ExceptionHandler(CicloEmAbertoException.class)
    public ResponseEntity<ErroResponse> handleConflict(CicloEmAbertoException ex) {
        return ResponseEntity.status(409).body(new ErroResponse(ex.getMessage()));
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ErroResponse> handleValidation(MethodArgumentNotValidException ex) {
        // retorna 400 com lista de campos invalidos
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErroResponse> handleGeneric(Exception ex) {
        log.error("Erro nao tratado", ex);
        return ResponseEntity.status(500).body(new ErroResponse("Erro interno"));
        // NUNCA expor stack trace na resposta
    }
}
```

---

## Referencias

- https://docs.spring.io/spring-boot/docs/current/reference/html/
- https://docs.spring.io/spring-data/jpa/docs/current/reference/html/
