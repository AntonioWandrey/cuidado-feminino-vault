---
tags:
  - sprint
  - frontend
  - calendario
  - ciclo
status: ativo
inicio: 2026-04-20
---

# Sprint 02 — Frontend do Ciclo Menstrual

> **Objetivo:** Integrar o backend do motor preditivo (já pronto desde Sprint 01) ao frontend, e implementar o CycleRing + CalendárioPage com código de cores real.
> Backend: ✅ pronto. Frontend: 🔄 a fazer.

---

## Contexto

O backend de Sprint 01 já entrega:
- `GET /api/v1/ciclos/previsao` → próxima menstruação, ovulação, período fértil, confiança
- `GET /api/v1/ciclos/calendario?meses=3` → projeção de fases por data

A `api.ts` precisa ser corrigida: `baseURL` ainda aponta para `/api/saude` (Sprint 00), deve ser `/api/v1`.

---

## Tarefas

### 🔧 Correções (dívida técnica)
- [ ] Corrigir `baseURL` em `frontend/src/services/api.ts` → `http://localhost:8080/api/v1`
- [ ] Adicionar tipos TypeScript para `PrevisaoCicloResponse` e `CalendarioCicloResponse` em `types/index.ts`

### 🏠 HomeScreen — Redesign com CycleRing
- [ ] Criar componente `CycleRing.tsx` em `components/`
  - SVG 240×240 com arcos de fase coloridos (coral/rose/blue/lavender)
  - Marcador do dia atual (círculo com borda roxa)
  - Texto central: dia do ciclo + fase atual
  - Conectar ao `GET /api/v1/ciclos/previsao`
- [ ] Adicionar stats row abaixo do anel: Próx. menstr. / Ovulação / Confiança
- [ ] Criar componente `PhaseTag.tsx` em `components/`
  - Badge colorido por fase com mapeamento de cores
- [ ] Adicionar card "Insight de hoje" (texto estático por fase, dinâmico no Sprint 03)
- [ ] Adicionar cards "Dicas para hoje" (energia/hidratação/sono — estático por fase)
- [ ] Substituir o acesso rápido atual pelo quick mood log (5 emojis → navega para Register)
- [ ] Adicionar CTA "Relatório para o médico" (navega para Report — desabilitado até Sprint 04)

### 📅 CalendárioPage — Redesign
> A página atual é uma agenda de consultas. Substituir pelo calendário de ciclo real.

- [ ] Reescrever `CalendarioPage.tsx` com grade mensal real
  - Conectar ao `GET /api/v1/ciclos/calendario?meses=3`
  - Código de cores: coral (menstruação), azul (fértil), dourado (ovulação)
  - Ponto dourado nos dias de ovulação
  - Borda roxa no dia atual
- [ ] Criar componente `BottomSheet.tsx` em `components/`
  - Painel deslizante ao clicar em um dia
  - Exibe: data, PhaseTag, dia do ciclo, sintomas registrados no dia
- [ ] Adicionar seção "Meses anteriores" com mini-grids em scroll horizontal
- [ ] Adicionar legenda de cores no rodapé do calendário
- [ ] Adicionar navegação entre meses (← →)

### 📋 RegisterPage — Página dedicada (substitui modal)
> O `SymptomForm` atual é um modal básico. Criar página completa como aba no BottomNav.

- [ ] Criar `RegisterPage.tsx` em `pages/`
  - Seção Fluxo menstrual: 4 opções com ícones de gotas (Leve/Moderado/Intenso/Muito intenso)
  - Seção Humor: 8 emojis selecionáveis
  - Seção Sintomas físicos: 8 sintomas com toggle de intensidade 3 níveis (Leve/Moderado/Intenso)
    - Cólica, Enxaqueca, Inchaço, Humor, Sono, Energia, TPM, Seios sensíveis
  - Seção Observações: textarea livre
  - Botão "Salvar registro" com toast de confirmação
  - Conectar ao backend existente `POST /api/saude/registrar` (adaptar payload)
- [ ] Adicionar "Registrar" como 3ª aba central no `BottomNav.tsx`
  - Botão circular destacado (roxo, elevado)
  - Ícone de `+`

### 🧭 Navegação
- [ ] Atualizar `BottomNav.tsx` para 5 abas: Início / Calendário / Registrar (centro) / Previsões / Perfil
- [ ] Atualizar `Index.tsx` para incluir a nova aba "register"
- [ ] Criar rota/estado para `InsightsPage` (placeholder — implementação no Sprint 03)

---

## Critérios de Aceite

- [ ] CycleRing exibe fase correta baseada em dados reais do backend
- [ ] Calendário colorido com dados reais de projeção
- [ ] BottomSheet mostra detalhes ao clicar em qualquer dia
- [ ] RegisterPage salva sintomas com sucesso e exibe toast
- [ ] BottomNav com 5 abas funcionando
- [ ] `npm test` sem falhas

---

## Referências
- [[Analise-Prototipo-2026-04-20]] — fonte das especificações visuais
- Backend Sprint 01: endpoints `/api/v1/ciclos/*` já disponíveis
- Design tokens: `#6B4C8C` (primary), `#FF7A7A` (coral), `#7EB8DA` (blue), `#F5C542` (gold)
