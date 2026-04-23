---
tags:
  - sprint
  - frontend
  - onboarding
  - insights
  - backend
status: planejado
inicio: previsto após Sprint 02
---

# Sprint 03 — Novas Telas: Onboarding + Previsões + Insight do Dia

> **Objetivo:** Implementar as telas que existem no protótipo mas ainda não existem no projeto: Onboarding, InsightsScreen e o endpoint de insight do dia no backend.

---

## Tarefas

### 🌸 OnboardingScreen
- [ ] Criar `OnboardingPage.tsx` em `pages/`
  - Slide 0 (splash): logo animado, título "Cuidado Feminino", subtítulo, botões "Começar" e "Já tenho conta"
  - Slide 1: ilustração do ciclo em SVG + "Entenda seu corpo"
  - Slide 2: mini-calendário colorido + "Previsões inteligentes"
  - Slide 3: card de relatório + "Conecte com seu médico"
  - Dots de paginação animados (largura variável)
  - Botão "Pular" nos slides intermediários
- [ ] Exibir Onboarding apenas na primeira vez (flag em `localStorage`)
- [ ] Ao concluir, navegar para Home

### 📊 InsightsPage (aba "Previsões")
- [ ] Criar `InsightsPage.tsx` em `pages/`
  - Cabeçalho: "Previsões" + "Baseado em X ciclos registrados"
  - Badge de confiança: ALTA / MÉDIA / BAIXA com ± dias (consumir `GET /api/v1/ciclos/previsao`)
  - Gráfico de barras de duração dos últimos ciclos (usar `recharts` já instalado)
    - Barra roxa = ciclos na média, barra lavanda = fora
    - Label com valor acima de cada barra
  - Card "Seu padrão": duração média / variação ± / regularidade
  - Card "Próximo ciclo": timeline horizontal de fases com datas previstas
  - Card "Sintomas mais frequentes": barras com % (consumir novo endpoint)
  - CTA de engajamento: "X dias sem registro esta semana → Registrar agora"
- [ ] Adicionar InsightsPage como aba "Previsões" no BottomNav (ícone de estrela)

### 🔌 Backend — Novos Endpoints (Sprint 03)

#### `GET /api/v1/ciclos/insight-do-dia`
- Retorna texto de insight baseado na fase atual da usuária
- Lógica: determinar fase pelo `GET /previsao`, retornar texto de um mapa fase→texto
- Response:
```json
{
  "fase": "FERTIL",
  "insight": "Você está no período mais fértil do ciclo. Seu estrogênio está no pico...",
  "dicas": [
    { "icone": "energia", "titulo": "Energia alta", "subtitulo": "Ótimo dia para exercícios intensos" },
    { "icone": "hidratacao", "titulo": "Hidratação", "subtitulo": "Beba pelo menos 2L de água hoje" },
    { "icone": "sono", "titulo": "Sono", "subtitulo": "Pode dormir um pouco menos esta semana" }
  ]
}
```

#### `GET /api/v1/ciclos/sintomas/frequentes?periodo=6`
- Calcula frequência % de cada sintoma nos últimos N ciclos
- Agrupa por `tipo_sintoma` da tabela de sintomas
- Response:
```json
[
  { "sintoma": "Energia alta", "percentual": 85 },
  { "sintoma": "Sem cólicas", "percentual": 72 }
]
```

### 🏠 Home — Completar integrações (Sprint 02 deixará pendente)
- [ ] Conectar "Insight de hoje" ao `GET /api/v1/ciclos/insight-do-dia`
- [ ] Conectar "Dicas para hoje" ao campo `dicas` do insight

---

## Critérios de Aceite

- [ ] Onboarding aparece apenas na primeira sessão
- [ ] InsightsPage exibe dados reais do backend
- [ ] Insight do dia retorna texto correto para cada uma das 5 fases
- [ ] Badge de confiança reflete nível real do motor preditivo
- [ ] `npm test` e testes de integração do backend sem falhas

---

## Referências
- [[Analise-Prototipo-2026-04-20]] — especificações visuais do InsightsScreen
- [[Sprint 02 - Frontend Ciclo]] — pré-requisito (BottomNav 5 abas)
- `recharts` já instalado na versão 2.15.4
