---
tags:
  - banco
  - modelagem
  - ciclo
atualizado: 2026-08-27
---

# Banco — modelagem de ciclo menstrual

## Escopo

O schema abaixo pertence ao backend remoto. A Entrega 2 usa `localStorage` e não grava ciclos no MySQL.

## Migrations

| Versão | Tabelas/função |
|---|---|
| V1 | registros de saúde iniciais |
| V2 | `ciclo_menstrual` e `previsao_ciclo` |
| V3 | conteúdo educativo, categorias e séries |
| V4 | registro de queixas |

Flyway está presente e as migrations devem ser a fonte do schema nos ambientes executáveis.

## Modelo remoto de ciclo

```text
ciclo_menstrual
├── id
├── usuario_id
├── data_inicio
├── data_fim
├── duracao_dias
├── fluxo
├── observacoes
├── criado_em
└── atualizado_em

previsao_ciclo
├── usuario_id
├── media_duracao_ciclo
├── desvio_padrao
├── ciclos_analisados
├── proxima_menstruacao
├── data_ovulacao
├── inicio_periodo_fertil
├── fim_periodo_fertil
└── confianca
```

## Correção semântica necessária

`duracao_dias` é calculada entre `data_inicio` e `data_fim`; portanto representa a duração do sangramento. A duração do ciclo deve ser calculada entre o início de um período e o início do próximo.

O motor remoto não deve usar `duracao_dias` como comprimento do ciclo. Antes da sincronização:

1. adicionar cálculo/coluna com semântica correta ou derivar intervalos por consulta;
2. testar ciclos regulares, irregulares e histórico curto;
3. migrar previsões antigas, se forem preservadas;
4. alinhar a regra com o [[ADR-005 - Calendario local-first e previsao no dispositivo]].

## Limitações

- `usuario_id = 1` é provisório e exclusivo do protótipo remoto;
- autenticação e chaves estrangeiras de usuário ainda precisam ser consolidadas;
- MariaDB é alvo compatível, mas as migrations foram validadas atualmente com MySQL.
