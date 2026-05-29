# Aurora CRM — LP v2 (Hubtown-inspired)

> Landing page editorial para o **Aurora CRM**, inspirada estruturalmente em [hubtown.co.in](https://hubtown.co.in/), com narrativa em capítulos via scroll horizontal.

---

## O que é

LP standalone (`index.html`) que conta a história da Aurora em 6 capítulos editoriais com scroll horizontal pinado, no estilo dos melhores sites do Awwwards / landing.love.

**Arquitetura visual:** dark editorial luxuoso com tipografia serif italica (Cormorant Garamond) + acento emerald (#00e38f), inspirada no preto/cream do Hubtown mas adaptada à identidade Aurora.

---

## Stack

- **HTML + CSS + JS vanilla** — sem build, abre direto no browser
- **Lenis 1.0.42** (CDN jsdelivr) — smooth scroll buttery
- **GSAP 3.12.2 + ScrollTrigger** (CDN cdnjs) — pin horizontal, scrub, snap, reveal
- **Google Fonts:** Cormorant Garamond (display serif) + DM Sans (body)
- **Inline SVG** para as ilustrações abstratas dos capítulos

---

## Estrutura da página (em ordem de scroll)

### 1. Preloader (0 → 100%)
- Número gigante em serif que conta de 0 a 100 (`gsap.to({val:0}, {val:100, ...})`)
- Barra de 1px que preenche em paralelo
- Label crossfade: "Carregando experiência" → "Pronto para explorar"
- Sai com `yPercent: -100, ease: expo.inOut`
- **Critical:** trava o Lenis (`lenis.stop()`) e libera quando termina (`lenis.start()`)

### 2. Nav (mix-blend-mode: difference)
- Logo "AURORA." à esquerda
- Pill SDR Online + Login + Menu à direita
- Usa `mix-blend-mode:difference` para auto-adaptar contraste em fundos variados

### 3. Intro Hero — 4 stat blocks scattered
Os 4 números principais distribuídos como uma constelação no grid 2×2:
- **+500 LEADS** — Gerenciados (top-left)
- **24/7 SDR** — Sempre ativo (top-right)
- **3× MAIS** — Agendamentos (bottom-left)
- **<40min SETUP** — Para começar (bottom-right)

Marca decorativa "Aurora · est. 2025" no centro. CTA editorial abaixo com indicador "scroll".

### 4. Words divider (marquee infinito)
Os 6 nomes dos capítulos (Inteligência · Automação · Integração · Excelência · Propósito · Legado) rolando horizontalmente com GSAP infinite repeat.

### 5. ⭐ HORIZONTAL SCROLL — Heart of the LP
**6 capítulos** que deslizam horizontalmente conforme o usuário rola verticalmente.

Cada capítulo (`.panel`) tem:
- **Número gigante** de fundo (01, 02, ...) em watermark com `font-size: 32vw`
- **Eyebrow:** "Capítulo NN · Inteligência" com linha decorativa
- **Headline** em 3 linhas grandes (cada linha com `overflow:hidden` para reveal animation)
- **Parágrafo** com a copy
- **CTA italic** com seta
- **Visualização SVG** abstrata à direita (orbe / fluxo / rede / geometria / barras / wordmark)

**Os 6 capítulos:**
1. **Inteligência** — "Construímos o futuro das vendas" (orbe + órbita)
2. **Automação** — "Lideramos a nova era do CRM" (curvas fluidas)
3. **Integração** — "Conectamos tudo em rede" (grafo de nós)
4. **Excelência** — "Construído para fazer o impossível" (geometria losango)
5. **Propósito** — "Vender com método e dados" (gráfico de barras + linha)
6. **Legado** — "E redefina o futuro do seu negócio" (wordmark AURORA)

### 6. Bottom horizontal nav (fixed durante h-section)
- Counter "01 / 06 · Inteligência" à esquerda
- Barra de progresso no centro
- Botões "← Anterior" / "Próximo →" à direita
- Aparece com `opacity 0→1` quando entra na seção horizontal
- Botões usam `lenis.scrollTo()` para navegar entre capítulos programaticamente

### 7. Final CTA + Form
- Headline gigante "Pronto para vender mais?" com mix de italic + outline stroke
- Lista de 5 benefícios
- Card de formulário com cantos decorativos (4 ângulos de 1px emerald)
- Form com inputs underline-only (sem caixa, super editorial)
- Estado de sucesso ao enviar

### 8. Footer
3 colunas: brand + tagline / produto / empresa. Linha inferior com copyright.

### 9. WhatsApp float (canto inferior direito)
Botão emerald circular com ícone SVG. Link para `wa.me/`.

### 10. Custom cursor
- Dot `mix-blend-mode:difference` (6px)
- Ring (42px) com lag maior
- Em hover: dot some, ring cresce + fica emerald
- Desabilitado em mobile via `@media (hover:hover)`

---

## Detalhes técnicos críticos

### Horizontal scroll com pin
```js
const distance = (PANELS - 1) * window.innerWidth;
const tween = gsap.to(hTrack, { x: -distance, ease: 'none' });

hScrollTrigger = ScrollTrigger.create({
  animation: tween,
  trigger: hSection,
  pin: true,
  scrub: 1.2,
  end: () => `+=${distance}`,
  snap: { snapTo: 1 / (PANELS - 1), duration: { min: .25, max: .5 } },
  onUpdate: self => updateHNav(...)
});
```

### Reveal de cada painel (containerAnimation)
Usa `containerAnimation: tween` no ScrollTrigger de cada painel — isso faz o trigger entender que o painel está dentro de uma animação horizontal e dispara o reveal quando ele "entra" pela direita.

```js
ScrollTrigger.create({
  trigger: panel,
  containerAnimation: tween,
  start: 'left 70%',
  onEnter: () => animatePanelIn(panel)
});
```

### Lenis + GSAP integration
```js
lenis.on('scroll', ScrollTrigger.update);
gsap.ticker.add(time => lenis.raf(time * 1000));
gsap.ticker.lagSmoothing(0);
```

### Mobile fallback
Se `window.innerWidth < 900`, o horizontal scroll é destruído (`hScrollTrigger.kill()`) e os painéis empilham verticalmente.

---

## Conteúdo (Aurora CRM)

- **Backend:** Python + FastAPI + PostgreSQL (port 8082)
- **Frontend app:** React + Vite + Tailwind (port 8083)
- **WhatsApp:** Evolution API Cloud
- **IA:** Anthropic Claude (`claude-haiku-4-5-20251001`)

### Funcionalidades reais do produto
1. Agente SDR com IA (responde WhatsApp 24/7)
2. Dashboard em tempo real
3. Agenda integrada (Google/Apple/Outlook via CalDAV)
4. WhatsApp Web nativo no CRM
5. Assistente IA interno
6. Pipeline de leads completo

### Métricas reais
- +500 leads gerenciados
- 24/7 agente ativo
- 3× mais agendamentos
- <40min para configuração inicial

---

## TODO / Próximos passos sugeridos

### Visuais
- [ ] Substituir SVGs abstratos por **screenshots reais do produto** em mockups de browser/celular
- [ ] Adicionar **vídeo loop** de demo do produto em um dos capítulos
- [ ] **Magnetic buttons** nos CTAs principais (botão se aproxima do cursor)
- [ ] **Text scramble** no preloader (letras random → texto final)

### Conteúdo
- [ ] Atualizar número do WhatsApp em `wa.me/5500000000000`
- [ ] Adicionar **logos de clientes** entre seções
- [ ] Capítulo de **depoimentos** entre capítulos 5 e 6
- [ ] **SEO completo:** OG image, twitter cards, sitemap

### Integração com backend
Endpoint para o form: `POST /auth/register` ou `POST /leads/landing-page`
```json
{
  "nome": "string",
  "email": "string",
  "empresa": "string",
  "segmento": "string",
  "whatsapp": "string"
}
```

### Performance
- [ ] Lazy load das SVGs dos capítulos não visíveis
- [ ] Preload das fonts críticas (Cormorant 300 + DM Sans 400)
- [ ] Comprimir grain SVG inline

---

## Arquivos

```
LP-Aurora.v2/
├── index.html       # LP completa, standalone
├── CONTEXT.md       # Este arquivo
└── README.md        # Instruções rápidas
```

## Como usar

1. Abra `index.html` direto no navegador
2. Para editar com Claude Code: abra a pasta no VS Code e peça:
   > "Leia o CONTEXT.md e me ajude a adicionar [feature]"

---

## Histórico de versões

- **v0:** LP simples com Three.js + IntersectionObserver
- **v1:** Cormorant Garamond + dark theme + sections empilhadas (sem horizontal scroll)
- **v2 (atual):** Estrutura Hubtown — preloader counter, intro com 4 stats scattered, marquee de palavras, **scroll horizontal com 6 capítulos pinados**, nav fixa prev/next, CTA editorial
