---
data: 2025-01-01
status: aceito
tags:
  - decisao
  - arquitetura
  - algoritmo
  - sprint-01
---

# ADR-003 - Media Móvel Ponderada para Previsão de Ciclo

## Contexto

O motor preditivo precisa calcular a próxima menstruação, ovulação e período fértil com base no histórico de ciclos. Era necessário um algoritmo que equilibrasse precisão e simplicidade, considerando que o histórico pode ser pequeno (1-3 ciclos) ou grande (12+).

## Decisão

Usar media móvel ponderada dos últimos 12 ciclos completos, com fallback de 28 dias quando não há histórico suficiente.

**Formula:**
- Ciclos mais recentes tem peso maior
- Ovulação = próxima menstruação - 14 dias (fase lútea padrão)
- Período fértil = ovulação - 5 dias ate ovulação + 1 dia

**Nível de confiança:**
- BAIXA: menos de 3 ciclos
- MEDIA: 3 a 5 ciclos
- ALTA: 6 ou mais ciclos

## Consequências

**Pros:**
- Simples de implementar e testar
- Adapta-se ao histórico real de cada usuária
- Nível de confiança comunica incerteza para a usuária
- Extensível: pode evoluir para machine learning no futuro

**Contras e Riscos:**
- Não considera variações hormonais ou condições clinicas como SOP
- A fase lútea de 14 dias e uma media, varia entre mulheres
- Com poucos ciclos a previsão pode ser imprecisa

## Alternativas Consideradas

| Alternativa              | Por que foi descartada                            |
| ------------------------ | ------------------------------------------------- |
| Media simples            | Não pondera ciclos recentes                       |
| Machine Learning         | Complexidade desproporcional para a fase atual    |
| Algoritmo de Knaus-Ogino | Muito rígido, não considera variações individuais |

## Referencias

- Doring, G.K. - The reliability of temperature records as a method of contraception
- https://helloclue.com/articles/cycle-a-z/how-period-tracking-apps-work
