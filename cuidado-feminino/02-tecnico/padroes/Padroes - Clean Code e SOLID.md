---
tags:
  - padroes
  - clean-code
  - solid
  - backend
---

# Padroes - Clean Code e SOLID

> Regras de codigo adotadas no projeto. Revisar antes de abrir PR.

---

## SOLID no Projeto

### S - Single Responsibility
Cada classe tem uma razao para mudar.

```java
// Correto - cada classe tem uma responsabilidade
public class MotorPreditivoCiclo { }   // so calcula previsoes
public class CicloService { }          // so orquestra regras de negocio
public class CicloController { }       // so lida com HTTP

// Errado - mistura responsabilidades
public class CicloService {
    public void calcularPrevisao() { }
    public ResponseEntity retornarHttp() { } // nao e responsabilidade do service
}
```

### O - Open/Closed
Aberto para extensao, fechado para modificacao.

```java
// Correto - novo algoritmo = nova implementacao, sem modificar o existente
public interface EstrategiaPrevisao {
    PrevisaoCiclo calcular(List<CicloMenstrual> historico);
}
public class MediaMovelPonderada implements EstrategiaPrevisao { }
public class RegressaoLinear implements EstrategiaPrevisao { }  // futuro
```

### D - Dependency Inversion
Dependa de abstracoes, nao de implementacoes.

```java
// Correto - injecao via interface
@Service
public class CicloService {
    private final CicloMenstrualRepository repository; // interface
    private final MotorPreditivoCiclo motor;
}
```

---

## Clean Code - Regras do Projeto

### Nomenclatura

| Elemento | Convencao | Exemplo |
|----------|-----------|---------|
| Classes | PascalCase | CicloMenstrual, MotorPreditivoCiclo |
| Metodos | camelCase com verbo | calcularPrevisao(), buscarUltimoCiclo() |
| Variaveis | camelCase descritivas | mediaDuracaoCiclo, ciclosAnalisados |
| Constantes | UPPER_SNAKE_CASE | DIAS_FASE_LUTEA, DURACAO_PADRAO |
| Packages | lowercase | com.cuidadofeminino.domain.service |

### Metodos

```java
// Correto - metodo pequeno, uma responsabilidade, maximo 20 linhas
private double calcularMediaPonderada(List<CicloMenstrual> ciclos) {
    // implementacao
}

// Errado - metodo que faz tudo com 100+ linhas
public PrevisaoCiclo calcularTudo(Long usuarioId) {
    // busca, calculo e persistencia misturados
}
```

### Comentarios

```java
// Correto - explica o PORQUE
// Fase lutea padrao = 14 dias (varia 12-16, mas 14 e a media clinica)
private static final int DIAS_FASE_LUTEA = 14;

// Errado - explica o QUE (o codigo ja diz isso)
// Soma 14 ao resultado
LocalDate ovulacao = proximaMenstruacao.minusDays(14);
```

### Tratamento de Erros

```java
// Correto - excecao especifica e significativa
throw new CicloNaoEncontradoException("Ciclo nao encontrado para usuario: " + usuarioId);

// Errado - excecao generica
throw new RuntimeException("Erro");
```

---

## Estrutura de Pacotes

```
com.cuidadofeminino
├── application
│   ├── dto/          DTOs de Request e Response
│   └── exception/    Excecoes customizadas + GlobalExceptionHandler
├── domain
│   ├── entity/       JPA Entities
│   ├── repository/   Spring Data interfaces
│   └── service/      Regras de negocio + Motor Preditivo
└── infrastructure
    ├── controller/   REST Controllers
    └── config/       Beans, CORS, Security
```

---

## Checklist de PR

- [ ] Nomes de classes, metodos e variaveis sao descritivos
- [ ] Nenhum metodo tem mais de 30 linhas
- [ ] Sem comentarios que explicam o que (so o porque)
- [ ] DTOs usados para entrada e saida, nunca expor entity diretamente
- [ ] Tratamento de excecoes com classes especificas
- [ ] Conventional commit aplicado
- [ ] Sem System.out.println - usar log.info/warn/error
