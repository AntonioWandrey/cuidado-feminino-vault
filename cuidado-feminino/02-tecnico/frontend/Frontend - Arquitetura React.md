---
tags:
  - frontend
  - arquitetura
  - react
  - typescript
---

# Frontend - Arquitetura React

> Estado real do frontend em 2026-04-15.

---

## Stack Real

| Tecnologia | Versao | Observacao |
|------------|--------|------------|
| React | 18.3.1 | |
| TypeScript | 5.8.3 | strict mode |
| Vite | 8.0.0 | build tool |
| Tailwind CSS | 3.4.17 | |
| Shadcn/UI | components.json | baseado em Radix UI |
| React Query | 5.83.0 | @tanstack/react-query |
| Axios | 1.13.6 | HTTP client |
| React Router | 6.30.1 | navegacao |
| React Hook Form | 7.61.1 | formularios |
| Zod | 3.25.76 | validacao de schemas |
| date-fns | 3.6.0 | manipulacao de datas |
| react-day-picker | 8.10.1 | componente de calendario |
| recharts | 2.15.4 | graficos (instalado, nao usado ainda) |
| Vitest | 4.1.0 | testes unitarios |

---

## Estrutura de Paginas

```
src/
├── pages/
│   ├── Index.tsx           Roteador de abas (estado: activeTab)
│   ├── HomePage.tsx        Aba Home — saudacao, dicas, acesso rapido
│   ├── CalendarioPage.tsx  Aba Calendario — AGENDA DE CONSULTAS (ver nota abaixo)
│   ├── TrilhasPage.tsx     Aba Trilhas — conteudo educacional
│   ├── PerfilPage.tsx      Aba Perfil — dados da usuaria
│   └── NotFound.tsx        404
├── components/
│   ├── BottomNav.tsx       Navegacao inferior com 4 abas
│   ├── NavLink.tsx         Item individual do BottomNav
│   ├── SymptomForm.tsx     Modal de registro de sintomas (Sprint 00)
│   ├── WellnessTipsCarousel.tsx Carrossel de dicas
│   └── ui/                 Componentes Shadcn/UI (button, card, dialog, etc.)
├── services/
│   └── api.ts              Instancia Axios — BASE URL INCORRETA (ver nota)
├── hooks/
│   └── use-mobile.tsx
├── lib/
│   ├── utils.ts            cn() helper
│   └── validations.ts      Schemas Zod
└── types/
    └── index.ts            Tipos globais
```

---

## Navegacao

O app usa um sistema de abas com estado local em `Index.tsx` (nao React Router):

```tsx
const [activeTab, setActiveTab] = useState("home");
// abas: "home" | "trilhas" | "calendario" | "perfil"
```

O React Router so tem duas rotas: `/` (Index) e `*` (NotFound).

---

## Estado das Paginas

### ✅ HomePage (Sprint 00 — funcional)
- Saudacao com horario do dia
- Botao "Como estou hoje?" abre SymptomForm
- Carrossel de dicas de bem-estar
- Acesso rapido: Trilhas, Calendario, Sintomas

### ✅ SymptomForm (Sprint 00 — funcional)
- Formulario de registro de sintomas (humor, sintomas fisicos, corrimento, outros sinais)
- Integrado com backend via `POST /api/saude/registrar`
- Alerta de UBS com link para Google Maps quando sintomas criticos
- Validacao com Zod

### ⚠️ CalendarioPage (Sprint 00/02 — atencao)
> **Esta nao e o calendario de ciclo menstrual planejado para Sprint 02.**
> A pagina atual e uma **agenda de consultas medicas** (consultas, exames, vacinas).
> O calendario de ciclo com codigo de cores (vermelho/azul/amarelo) ainda precisa ser implementado.
> Opcoes para Sprint 02:
> - Substituir esta pagina pelo calendario de ciclo
> - Criar nova aba "Ciclo" e renomear esta para "Agenda"

### ⏳ TrilhasPage (conteudo pendente)
- Estrutura criada, sem conteudo real ainda

### ⏳ PerfilPage (conteudo pendente)
- Estrutura criada, sem dados reais ainda

---

## Divida Tecnica: api.ts

```typescript
// ESTADO ATUAL (errado) — aponta para endpoint do Sprint 00
const api = axios.create({
  baseURL: 'http://localhost:8080/api/saude',
});

// CORRETO — deve apontar para os novos endpoints do Sprint 01
const api = axios.create({
  baseURL: 'http://localhost:8080/api/v1',
});
```

Os endpoints do Sprint 01 ainda nao estao integrados no frontend.

---

## Endpoints do Backend Disponiveis (Sprint 01)

```typescript
// Registrar inicio de ciclo
POST /api/v1/ciclos
Body: { dataInicio: "2026-04-15", fluxo: "MODERADO", observacoes?: "..." }

// Encerrar ciclo
PATCH /api/v1/ciclos/{id}/encerrar
Body: { dataFim: "2026-04-20", fluxo?: "LEVE", observacoes?: "..." }

// Listar ciclos
GET /api/v1/ciclos

// Obter previsao
GET /api/v1/ciclos/previsao

// Obter calendario de projecoes
GET /api/v1/ciclos/calendario?meses=3
```

---

## Codigo de Cores do Calendario (Sprint 02 — a implementar)

| Cor | Significado |
|-----|-------------|
| Vermelho | Dias de menstruacao |
| Azul | Periodo fertil |
| Amarelo | Ovulacao |

---

## Como Rodar

```bash
cd frontend
npm install
npm run dev    # http://localhost:5173
npm test       # vitest run
```
