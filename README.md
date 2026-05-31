# Site_Cafe_e_Cia_-KIRO-
# ☕ Café & Cia — Site Institucional com Integrações Reais

Site institucional de uma cafeteria local brasileira, construído via **Kiro** (IDE AI-powered) que gera automaticamente código de produção utilizando React 19, TypeScript 5.9, Vite 7 e Tailwind CSS 3.4. Demonstra a aplicação prática de padrões web (HTML semântico, CSS moderno e JavaScript avançado) dentro de uma abordagem low-code inteligente.

**Ferramenta orquestradora:** [Kiro](https://kiro.dev) — IDE AI-powered (low-code inteligente)  
**Trabalho:** Padrões Web para No Code e Low Code  
**Instituição:** UniFECAF  
**Aluno:** Diogo de Lima Braga  
**Curso:** Inteligência Artificial e Automação Digital  
**URL em produção:** https://cafe-e-cia-diogo.netlify.app

---

## Início Rápido

```bash
cd app
npm install
npm run dev        # http://localhost:5173
```

| Comando | Descrição |
|---------|-----------|
| `npm run dev` | Servidor de desenvolvimento (Vite hot reload) |
| `npm run build` | Build de produção (`tsc -b` + Vite) |
| `npm run preview` | Preview do build local |
| `npm run lint` | ESLint |

---

## Estrutura do Projeto

```
app/
├── src/
│   ├── sections/              # 7 seções da página (Header, Hero, Sobre, etc.)
│   ├── pages/
│   │   └── Home.tsx           # Composição de todas as seções
│   ├── components/
│   │   ├── ui/                # 40+ componentes shadcn/ui (Radix-based)
│   │   └── WhatsAppButton.tsx # Botão flutuante reutilizável
│   ├── data/
│   │   ├── menuData.ts        # Dados do cardápio (tipados)
│   │   └── agendaData.ts      # Sistema de disponibilidade (10 mesas)
│   ├── hooks/
│   │   └── useScrollReveal.ts # Intersection Observer + contadores animados
│   ├── lib/
│   │   ├── integrations.ts    # Config central de TODAS as 7 integrações
│   │   ├── google-apps-script.js # Script completo do Google Apps Script
│   │   └── utils.ts           # Utilitários (cn, clsx, tailwind-merge)
│   ├── index.css              # Estilos globais + variáveis CSS + animações
│   └── App.tsx                # Root com React Router
├── index.html                 # Entry point
├── vite.config.ts             # Configuração Vite
├── tailwind.config.js         # Configuração Tailwind + tema shadcn
└── postcss.config.js          # PostCSS
```

---

## Seções do Site (7)

| Seção | Arquivo | Funcionalidade | Integrações |
|-------|---------|----------------|-------------|
| Header | `src/sections/Header.tsx` | Navegação fixa, menu hamburger mobile, skip link | — |
| Hero | `src/sections/Hero.tsx` | Banner com efeito typewriter e CTA | — |
| Sobre | `src/sections/Sobre.tsx` | História com contadores animados (requestAnimationFrame) | — |
| Cardápio | `src/sections/Cardapio.tsx` | Filtros dinâmicos, modal de detalhes, imagens reais | Unsplash CDN |
| Galeria | `src/sections/Galeria.tsx` | Grid de fotos com lightbox e navegação | Unsplash CDN |
| Contato | `src/sections/Contato.tsx` | Formulário completo com disponibilidade em tempo real | EmailJS, Google Sheets, Google Calendar, WhatsApp |
| Footer | `src/sections/Footer.tsx` | Links funcionais e informações de contato | WhatsApp, Google Maps |

**Botão WhatsApp** (`src/components/WhatsAppButton.tsx`): flutuante em todas as páginas.

---

## 7 Integrações Reais (custo $0)

Todas configuradas em `src/lib/integrations.ts` (Single Source of Truth).

### 1. 📧 EmailJS — Envio de Email Real

Envia emails diretamente do front-end sem servidor SMTP.

| Campo | Valor |
|-------|-------|
| Service ID | `service_yqob54v` |
| Template ID | `template_cdjsu4k` |
| Destinatário | `diogo.braga.230831@a.fecaf.com.br` |

**Para configurar com suas credenciais:**
1. Acesse https://www.emailjs.com → crie conta gratuita
2. My Account → API Keys → copie a Public Key
3. Email Services → adicione seu provedor → copie o Service ID
4. Email Templates → crie template → copie o Template ID
5. Edite `EMAILJS_CONFIG` em `src/lib/integrations.ts`

### 2. 📊 Google Sheets + Apps Script — Planilha Automática

Cada reserva é salva automaticamente em planilha Google Sheets via webhook.

| Método | Função |
|--------|--------|
| POST | Salva reserva na planilha + cria evento no Calendar |
| GET | Retorna disponibilidade de mesas em tempo real |

**Para implantar:**
1. Acesse https://script.google.com → Novo projeto
2. Cole o conteúdo de `src/lib/google-apps-script.js`
3. Salve (Ctrl+S)
4. Implantar → Nova implantação → App da Web → Acesso: Qualquer pessoa
5. Copie a URL e cole em `GOOGLE_SHEETS_CONFIG.WEBHOOK_URL` no `integrations.ts`

**Dados salvos:** Timestamp, nome, email, telefone, tipo, data, horário, pessoas, mensagem, status.

### 3. 📅 Google Calendar — Eventos Automáticos

Cada reserva confirmada cria automaticamente um evento no Google Calendar.

- **Trigger:** automático ao salvar reserva (dentro do Apps Script)
- **Dados do evento:** nome do cliente, data, horário, nº de pessoas, localização
- **Calendário:** "Cafe & Cia - Reservas" (criado automaticamente)

Não requer configuração extra — funciona junto com o Apps Script do item 2.

### 4. 💬 WhatsApp Business — Atendimento Direto

| Campo | Valor |
|-------|-------|
| Número | +55 47 99613-6122 |
| Formato | `wa.me/5547996136122?text=mensagem` |
| Mensagem padrão | "Olá! Vim pelo site da Café & Cia..." |

**Para alterar:**
- Edite `WHATSAPP_CONFIG` em `src/lib/integrations.ts`
- `NUMBER`: formato DDI + DDD + número (sem espaços/traços)
- Componente visual: `src/components/WhatsAppButton.tsx`

### 5. 🗺️ Google Maps Embed — Mapa Real

Mapa interativo embedado na seção de contato (gratuito, sem API key).

| Campo | Valor |
|-------|-------|
| Endereço | Rua das Flores, 123 - Centro, São Paulo, SP |
| Método | iframe embed (sem API key paga) |

**Para alterar o endereço:**
1. Acesse Google Maps → busque o endereço
2. Compartilhar → Incorporar mapa → copie a URL do `src`
3. Cole em `MAPS_CONFIG.EMBED_URL` no `integrations.ts`

### 6. 📸 Unsplash CDN — Imagens Reais

Galeria e cardápio usam imagens de alta qualidade do Unsplash com otimização.

- **Parâmetros:** `?w=800&q=80` (largura e qualidade)
- **Galeria:** 6 imagens configuradas em `GALLERY_IMAGES` no `integrations.ts`
- **Cardápio:** imagens em `src/data/menuData.ts`
- **Alt text:** descrições detalhadas para acessibilidade

### 7. 🪑 Sistema de Agendamento — 10 Mesas em Tempo Real

| Campo | Valor |
|-------|-------|
| Total de mesas | 10 |
| Horários | Seg-Sex 7h-20h, Sáb 8h-20h, Dom 8h-14h |
| Slots | Intervalos de 1 hora |
| Status visual | 🟢 Disponível · 🟡 Poucas mesas · 🔴 Lotado |
| Fonte de dados | Google Sheets via Apps Script (tempo real) com fallback local |

**Configuração:** `src/data/agendaData.ts` (horários, total de mesas, lógica de disponibilidade).

---

## Dados do Cardápio

Arquivo: `src/data/menuData.ts`

**Para adicionar um item:**

```typescript
{
  id: 13,
  nome: 'Novo Item',
  descricao: 'Descrição do item',
  preco: 15.00,
  categoria: 'cafes', // 'cafes' | 'bebidas' | 'doces' | 'salgados'
  imagem: 'https://images.unsplash.com/photo-xxx?w=400&q=80',
  destaque: true,
  tempoPreparo: '5 min',
  alergenos: ['lactose', 'gluten']
}
```

**Categorias disponíveis:** Cafés Especiais, Bebidas, Doces & Sobremesas, Salgados & Lanches.

**Depoimentos:** array `depoimentos` no mesmo arquivo (nome, texto, nota, data, avatar).

---

## Personalizações CSS

| Customização | Técnica | Arquivo |
|--------------|---------|---------|
| 6 animações @keyframes | fadeInUp, fadeInLeft, float, pulse, shimmer, spin-slow | `src/index.css` |
| Glassmorphism | backdrop-filter + rgba | `src/index.css` |
| Variáveis CSS (design tokens) | Custom Properties no :root | `src/index.css` |
| Scrollbar personalizada | ::-webkit-scrollbar | `src/index.css` |
| Gradientes customizados | linear-gradient com cores da marca | `src/index.css` |
| Botão com efeito shimmer | ::before + animation no hover | `src/index.css` |

**Cores da marca (variáveis CSS):**
```css
--cor-primaria: #6F4E37;
--cor-secundaria: #D4A574;
--cor-fundo: #FDF8F3;
--cor-texto: #2D2A26;
```

---

## Personalizações JavaScript

| Funcionalidade | API/Técnica | Arquivo |
|----------------|-------------|---------|
| Scroll Reveal | Intersection Observer API | `src/hooks/useScrollReveal.ts` |
| Contadores animados | requestAnimationFrame + easing | `src/hooks/useScrollReveal.ts` |
| Efeito Typewriter | setTimeout recursivo + state | `src/sections/Hero.tsx` |
| Validação de formulário | Regex + aria-invalid | `src/sections/Contato.tsx` |
| Máscara de telefone | replace + regex dinâmico | `src/sections/Contato.tsx` |
| Fetch para APIs reais | fetch + Promise.all (paralelo) | `src/sections/Contato.tsx` |
| Disponibilidade em tempo real | fetch + useEffect + state | `src/sections/Contato.tsx` |

---

## Responsividade

Abordagem **mobile-first** com breakpoints do Tailwind:

| Breakpoint | Largura | Adaptações |
|------------|---------|------------|
| Mobile | < 768px | Menu hamburger, grid 1 coluna, fontes reduzidas, botões full-width |
| Tablet | 768px – 1023px | Grid 2 colunas, navegação expandida parcial |
| Desktop | 1024px+ | Grid 3-4 colunas, layout lado a lado, hover effects |

---

## Acessibilidade (WCAG 2.1)

| Recurso | Implementação |
|---------|---------------|
| Skip Link | `<a href="#conteudo-principal">` |
| ARIA Labels | Em todos os elementos interativos |
| Roles Semânticas | banner, navigation, main, contentinfo, region |
| Focus Visible | Outline visível com focus-visible |
| Contraste | Texto #2D2A26 sobre fundo #FDF8F3 (ratio 9.5:1) |
| Reduced Motion | `@media (prefers-reduced-motion: reduce)` |
| Alto Contraste | `@media (prefers-contrast: high)` |
| Formulário Acessível | aria-invalid, aria-describedby, aria-required |

---

## Como Modificar

| Quero... | Onde mexer |
|----------|-----------|
| Alterar textos/conteúdo | Seção correspondente em `src/sections/` |
| Mudar cardápio | `src/data/menuData.ts` |
| Trocar imagens | URLs do Unsplash em `menuData.ts` ou `integrations.ts` |
| Alterar cores/tema | Variáveis CSS em `src/index.css` e `tailwind.config.js` |
| Mudar horários de funcionamento | `src/data/agendaData.ts` → `HORARIOS_POR_DIA` |
| Alterar total de mesas | `src/data/agendaData.ts` → `TOTAL_MESAS` |
| Configurar integrações | `src/lib/integrations.ts` |
| Alterar número WhatsApp | `WHATSAPP_CONFIG.NUMBER` em `integrations.ts` |
| Mudar endereço do mapa | `MAPS_CONFIG.EMBED_URL` em `integrations.ts` |
| Adicionar nova página | Criar em `src/pages/` e adicionar rota em `App.tsx` |
| Adicionar componente UI | `npx shadcn@latest add [componente]` |
| Alterar animações | `src/index.css` (seção @keyframes) |

---

## Tech Stack

| Tecnologia | Versão | Justificativa |
|------------|--------|---------------|
| **Kiro** | 2026 | IDE AI-powered que orquestra a geração de código via linguagem natural (low-code inteligente) |
| React | 19.x | Componentização visual similar a blocos no-code |
| TypeScript | 5.9.x | Tipagem estática que previne erros |
| Vite | 7.2.x | Build ultrarrápido, hot reload instantâneo |
| Tailwind CSS | 3.4.x | Classes utilitárias que simulam painéis de estilo visual |
| shadcn/ui | — | 40+ componentes prontos (equivalente a widgets no-code) |
| React Router | 7.x | SPA com roteamento |
| React Hook Form + Zod | — | Formulários com validação tipada |
| Lucide React | — | Ícones SVG |
| Recharts | — | Gráficos |
| Sonner | — | Toasts/notificações |
| Embla Carousel | — | Carrossel |

---

## Deploy (Netlify)

O site está hospedado no Netlify (free tier, custo $0) com HTTPS automático.

```bash
npm run build    # gera pasta dist/
```

Deploy automático ao fazer push. Configuração em `.netlify/netlify.toml`.

---

## Fluxo de Dados (Formulário de Contato)

```
Usuário preenche formulário
  → Validação (regex + aria-invalid)
  → Promise.all()
  → [EmailJS] email real enviado
  → [Google Sheets] lead salvo na planilha
  → [Google Calendar] evento criado automaticamente
  → Toast de confirmação (Sonner)
```

---

## Links Úteis

| Recurso | URL |
|---------|-----|
| **Site em produção** | https://cafe-e-cia-diogo.netlify.app |
| **Kiro (IDE AI-Powered)** | https://kiro.dev |
| React | https://react.dev |
| TypeScript | https://www.typescriptlang.org/docs |
| Vite | https://vite.dev |
| Tailwind CSS | https://tailwindcss.com/docs |
| shadcn/ui | https://ui.shadcn.com |
| React Router | https://reactrouter.com |
| EmailJS | https://www.emailjs.com/docs |
| Google Apps Script | https://developers.google.com/apps-script |
| Google Calendar API | https://developers.google.com/calendar |
| WhatsApp Business | https://business.whatsapp.com |
| Google Maps Embed | https://developers.google.com/maps/documentation/embed |
| Unsplash | https://unsplash.com/developers |
| Lucide Icons | https://lucide.dev |
| React Hook Form | https://react-hook-form.com |
| Zod | https://zod.dev |
| Netlify | https://docs.netlify.com |
| MDN — HTML | https://developer.mozilla.org/pt-BR/docs/Web/HTML |
| MDN — CSS | https://developer.mozilla.org/pt-BR/docs/Web/CSS |
| MDN — JavaScript | https://developer.mozilla.org/pt-BR/docs/Web/JavaScript |
| WCAG 2.1 | https://www.w3.org/WAI/WCAG21/quickref |
| WAI-ARIA Practices | https://www.w3.org/WAI/ARIA/apg |
| Intersection Observer API | https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API |

---

## Sobre o Kiro

O **Kiro** é um ambiente de desenvolvimento inteligente (IDE AI-powered) que funciona como plataforma low-code: o desenvolvedor descreve requisitos em linguagem natural e o Kiro gera, refatora e mantém o código automaticamente. Neste projeto, o Kiro orquestrou toda a construção utilizando React 19, TypeScript 5.9, Vite 7 e Tailwind CSS 3.4 — combinando a velocidade de plataformas no-code com controle total sobre o código-fonte.

**Por que o Kiro?**
- Gera código de produção a partir de descrições em linguagem natural
- Mantém padrões de acessibilidade, performance e boas práticas automaticamente
- Permite integrações ilimitadas (fetch, webhooks, SDKs) sem depender de plugins
- Oferece controle total sobre HTML semântico, CSS e JavaScript — diferente de plataformas no-code tradicionais
- Custo zero para o desenvolvedor

Saiba mais: https://kiro.dev

---

## Licença

Projeto acadêmico — UniFECAF, Abril de 2026.

> ☕ Café & Cia — Onde cada xícara conta uma história

