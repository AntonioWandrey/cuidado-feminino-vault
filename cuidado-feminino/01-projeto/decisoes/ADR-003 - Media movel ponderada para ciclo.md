---
data: 2025-01-01
status: aceito
tags:
  - decisao
  - arquitetura
  - algoritmo
  - sprint-01
---

# ADR-003 - Media Movel Ponderada para Previsao de Ciclo

## Contexto

O motor preditivo precisa calcular a proxima menstruacao, ovulacao e periodo fertil com base no historico de ciclos. Era necessario um algoritmo que equilibrasse precisao e simplicidade, considerando que o historico pode ser pequeno (1-3 ciclos) ou grande (12+).

## Decisao

Usar media movel ponderada dos ultimos 12 ciclos completos, com fallback de 28 dias quando nao ha historico suficiente.

**Formula:**
- Ciclos mais recentes tem peso maior
- Ovulacao = proxima menstruacao - 14 dias (fase lutea padrao)
- Periodo fertil = ovulacao - 5 dias ate ovulacao + 1 dia

**Nivel de confianca:**
- BAIXA: menos de 3 ciclos
- MEDIA: 3 a 5 ciclos
- ALTA: 6 ou mais ciclos

## Consequencias

**Pros:**
- Simples de implementar e testar
- Adapta-se ao historico real de cada usuaria
- Nivel de confianca comunica incerteza para a usuaria
- Extensivel: pode evoluir para machine learning no futuro

**Contras e Riscos:**
- Nao considera variacoes hormonais ou condicoes clinicas como SOP
- A fase lutea de 14 dias e uma media, varia entre mulheres
- Com poucos ciclos a previsao pode ser imprecisa

## Alternativas Consideradas

| Alternativa | Por que foi descartada |
|-------------|------------------------|
| Media simples | Nao pondera ciclos recentes |
| Machine Learning | Complexidade desproporcional para a fase atual |
| Algoritmo de Knaus-Ogino | Muito rigido, nao considera variacoes individuais |

## Referencias

- Doring, G.K. - The reliability of temperature records as a method of contraception
- https://helloclue.com/articles/cycle-a-z/how-period-tracking-apps-work
