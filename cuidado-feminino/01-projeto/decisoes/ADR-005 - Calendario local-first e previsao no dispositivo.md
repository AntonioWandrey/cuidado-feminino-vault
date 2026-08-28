---
data: 2026-08-27
status: aceito
tags:
  - decisao
  - arquitetura
  - calendario
  - offline
  - privacidade
---

# ADR-005 — Calendário local-first e previsão no dispositivo

## Contexto

A Entrega 2 exige registro local de início e término, consulta no calendário e previsão do próximo ciclo. A demonstração precisa funcionar sem backend, rede ou banco.

## Decisão

Usar `localStorage` como fonte de verdade do calendário preliminar, sob a chave versionada:

```text
cuidado-feminino:ciclos:v1
```

O cliente valida, persiste, consulta e calcula a previsão no dispositivo.

### Regra de previsão

- ordenar registros por data de início;
- calcular intervalos entre inícios consecutivos;
- usar a média simples dos últimos seis intervalos;
- usar fallback de 28 dias quando não houver intervalo suficiente;
- avançar a previsão até uma data futura;
- ovulação estimada: próxima menstruação menos 14 dias;
- janela fértil estimada: cinco dias antes até um dia depois;
- confiança baixa com menos de 3 intervalos, média com 3 a 5 e alta a partir de 6.

## Consequências

### Positivas

- atende a entrega sem infraestrutura externa;
- persiste após recarga e reabertura do WebView;
- cálculo usa duração real do ciclo, não duração do sangramento;
- funções são pequenas e testáveis.

### Negativas e riscos

- `localStorage` não é criptografado;
- os dados não têm backup nem sincronização;
- limpar o armazenamento do app apaga o histórico;
- não há compartilhamento entre dispositivos;
- a previsão não é diagnóstico nem método contraceptivo.

## Alternativas consideradas

| Alternativa | Motivo para não escolher agora |
|---|---|
| API online-first | Não atende a demonstração offline e exige banco disponível |
| Capacitor Preferences | Boa evolução nativa, mas adicionaria plugin e sincronização perto do prazo |
| SQLite | Mais robusto, porém desproporcional ao protótipo e exige migrations locais |

## Evolução

Avaliar armazenamento seguro/criptografado, exportação e exclusão local. Qualquer sincronização seguirá o [[ADR-006 - Sincronizacao futura com Spring e banco relacional]].
