---
tags:
  - prototipo
  - analise
  - backlog
  - produto
data: 2026-04-20
---

# Análise do Protótipo HTML — Cuidado Feminino

> Arquivo analisado: `Cuidado Feminino.html` + `screens/` (8 telas JSX)
> Data da análise: 2026-04-20
> Objetivo: mapear o que o protótipo tem que ainda NÃO existe no projeto.

---

## Telas do Protótipo

| # | Tela | No projeto? | Status |
|---|------|-------------|--------|
| 1 | Onboarding (splash + 3 slides) | ❌ Não existe | A criar |
| 2 | Home (com CycleRing) | ⚠️ Existe, mas diferente | Redesign |
| 3 | Calendário (com BottomSheet + mini-grids) | ⚠️ Existe, mas diferente | Redesign |
| 4 | **Register** (página dedicada de sintomas) | ⚠️ Só modal hoje | Nova página |
| 5 | **Insights** (Previsões — estatísticas visuais) | ❌ Não existe | A criar |
| 6 | **Report** (Relatório Médico PDF) | ❌ Não existe | A criar |
| 7 | Perfil (com planos + LGPD) | ⚠️ Existe básico | Ampliar |
| 8 | **Doctor** (portal desktop médico) | ❌ Não existe | A criar |

---

## Novidades por Categoria

### 🆕 Telas Completamente Novas

#### 1. OnboardingScreen
- Splash com logo + botões "Começar" e "Já tenho conta"
- 3 slides com ilustrações SVG: Entenda seu corpo / Previsões inteligentes / Conecte com seu médico
- Dots de paginação animados
- Botão "Pular" e navegação sequencial

#### 2. InsightsScreen (aba "Previsões")
- Badge de confiança (ALTA / MÉDIA / BAIXA) com ± dias
- Gráfico de barras: duração dos últimos 6 ciclos
- Card "Seu padrão": duração média, variação, regularidade
- Timeline horizontal do próximo ciclo (fases coloridas com datas)
- Gráfico de barras: sintomas mais frequentes com %
- CTA: "Quanto mais você registra, mais preciso fica!"

#### 3. ReportScreen (Relatório Médico)
- Cabeçalho estilo PDF com gradiente roxo
- Dados da paciente + período coberto
- Tabela de histórico de ciclos (6 ciclos: mês/início/duração/status)
- Mini-gráfico de regularidade com destaque de ciclos normais/fora
- Análise clínica com alertas (✓ verde / ℹ azul / ⚠ vermelho)
- Seção "Sintomas mais frequentes" com barras de %
- Botões: 📤 Compartilhar + ⬇ Baixar PDF
- **Paywall overlay** para plano Free (desfoque + botão "Assinar Pro · R$19,90/mês")

#### 4. DoctorScreen (portal desktop — Plano Clínica)
- Layout desktop com sidebar + conteúdo principal
- Sidebar: lista de pacientes com fase atual, dia do ciclo, badge de alerta
- Campo de busca de pacientes
- Cabeçalho da paciente selecionada com PhaseTag e dia do ciclo
- Botão "Gerar PDF" por paciente
- Gráfico de histórico de ciclos (normal / irregular / levemente fora)
- Timeline do ciclo atual com marcador do dia
- Painel de alertas clínicos (severidade alta/média por paciente)
- Tela fora do frame do celular — viewport completo

---

### 🔄 Redesigns de Telas Existentes

#### Home (redesign completo)
**O que adicionar:**
- **CycleRing** SVG: anel com arcos de fase, marcador do dia atual, texto central com dia/fase
- Stats row abaixo do anel: Próx. menstr. / Ovulação / Confiança
- Card "Insight de hoje" — texto gerado baseado na fase atual
- Cards "Dicas para hoje" — dicas de energia, hidratação, sono por fase (3 cards)
- Quick mood log com 5 emojis (atalho para RegisterScreen)
- CTA "Relatório para o médico" com contador de dias até consulta

#### CalendarioPage (melhorias)
**O que adicionar:**
- BottomSheet ao clicar em um dia: data + PhaseTag + dia do ciclo + sintomas registrados
- Seção "Meses anteriores" com mini-grids horizontais (scroll)
- Ponto dourado nos dias de ovulação
- Legenda de cores no rodapé

#### PerfilPage (ampliações)
**O que adicionar:**
- Seção "Meu ciclo" com 3 stats: duração média / último ciclo / ciclos registrados
- Comparação de 3 planos (Free / Pro / Clínica) com toggle "Ver planos"
- Seção LGPD completa:
  - Toggle "Dados anônimos para pesquisa" (contribui com estudos científicos)
  - Botão "Exportar meus dados"
  - Botão "Excluir minha conta" (ação destrutiva — confirmação obrigatória)

---

### 🧩 Novos Componentes Compartilhados

| Componente | Descrição |
|------------|-----------|
| `CycleRing` | SVG com arcos de fase coloridos, marcador do dia atual, texto central |
| `PhaseTag` | Badge colorido por fase: Menstruação / Folicular / Fértil / Ovulação / Lútea |
| `BottomSheet` | Painel deslizante de baixo com overlay e indicador de arraste |

---

### 💼 Modelo de Negócio Definido no Protótipo

| Plano | Preço | Features |
|-------|-------|---------|
| **Free** | Grátis | Rastreio básico, Calendário, Previsões simples |
| **Pro** | R$ 19,90/mês | + Relatórios clínicos, Insights avançados, Exportar PDF |
| **Clínica** | R$ 49,90/mês | + Painel do médico, Multi-pacientes, Alertas clínicos |

---

### 🔌 Novos Endpoints Backend Necessários

| Endpoint | Sprint | Descrição |
|----------|--------|-----------|
| `GET /api/v1/ciclos/insight-do-dia` | 03 | Texto do insight baseado na fase atual |
| `GET /api/v1/ciclos/sintomas/frequentes` | 03 | Frequência % de cada sintoma por período |
| `POST /api/v1/relatorios/gerar` | 04 | Gerar relatório clínico (retorna JSON ou PDF) |
| `GET /api/v1/usuarios/exportar-dados` | 04 | Exportar dados pessoais (LGPD Art. 18) |
| `POST /api/v1/usuarios/aceitar-pesquisa` | 04 | Consentimento para dados anônimos |
| `DELETE /api/v1/usuarios/excluir-conta` | 04 | Direito ao esquecimento (LGPD Art. 18) |
| `GET /api/v1/medico/pacientes` | 05 | Lista de pacientes vinculados (Plano Clínica) |
| `GET /api/v1/medico/pacientes/{id}/ciclos` | 05 | Histórico de ciclos de uma paciente |
| `GET /api/v1/medico/alertas` | 05 | Alertas clínicos agregados de todas as pacientes |

---

## Design System do Protótipo (tokens CF)

```js
// Paleta de cores do protótipo (para replicar no Tailwind/CSS)
primary:      #6B4C8C   // roxo
rose:         #E8B4C8   // rosa
coral:        #FF7A7A   // vermelho suave
blue:         #7EB8DA   // azul
gold:         #F5C542   // dourado (ovulação)
green:        #4CAF7D   // verde
lavender:     #C5A8E0   // lavanda (fase lútea)

// Tipografia
fontHead: 'Playfair Display' (títulos)
fontBody: 'DM Sans' (corpo)
```

---

## Próximos Passos — Sprints Propostos

Ver: [[Sprint 02 - Frontend Ciclo]] e [[Sprint 03 - Novas Telas]]
