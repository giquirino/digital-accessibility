# Relatório de Acessibilidade — Resultados do axe DevTools

> Auditoria realizada com axe DevTools (Deque Systems) — extensão para Chrome  
> Motor: axe-core 4.10 · ~90 regras de acessibilidade · Data: Abril/2025

---

## picpay.com — Resumo

| Severidade | Quantidade | Elementos |
|---|---|---|
| 🔴 Critical | 3 issues | 6 |
| 🟠 Serious | 4 issues | 19 |
| 🟡 Moderate | 2 issues | 7 |
| Total | **9 issues** | **32** |

---

### Falhas encontradas (9 issues, 32 elementos afetados)

| # | Severidade | Rule ID (axe) | Critério WCAG | Erro | Elementos |
|---|---|---|---|---|---|
| 1 | 🔴 Critical | `button-name` | 4.1.2 — Name, Role, Value | Botões sem nome acessível | 3 |
| 2 | 🔴 Critical | `link-name` | 2.4.4 — Link Purpose | Links sem texto discernível | 3 |
| 3 | 🟠 Serious | `image-alt` | 1.1.1 — Non-text Content | Imagens sem atributo `alt` | 4 |
| 4 | 🟠 Serious | `color-contrast` | 1.4.3 — Contrast (Minimum) | Contraste insuficiente | 2 |
| 5 | 🟠 Serious | `label-content-name-mismatch` | 2.5.3 — Label in Name | Label visível ≠ nome acessível | 2 |
| 6 | 🟠 Serious | `target-size` | 2.5.5 — Target Size | Alvos de toque < 24×24px | 13 |
| 7 | 🟡 Moderate | `heading-order` | 2.4.6 — Headings and Labels | Hierarquia de headings fora de ordem | 5 |
| 8 | 🟡 Moderate | `region` | Best Practice | Conteúdo fora de landmarks (`<main>`, `<nav>`) | 2 |
| 9 | — | `landmark-one-main` | Best Practice | Nenhum `<main>` encontrado na página | 1 |

---

### Detalhes dos erros com exemplos de código

**`button-name` — Botões sem nome acessível** (🔴 Critical)

O axe DevTools destaca o elemento na página com borda vermelha e mostra o seletor CSS exato.

```html
<!-- ❌ Encontrado no picpay.com -->
<!-- Seletor: div.teaser__dots > button.teaser__dot -->
<button class="teaser__dot active" data-gtm-button-decorated="true">
</button>

<!-- ✅ Correção recomendada pelo axe -->
<button class="teaser__dot active" aria-label="Slide 1 de 5: Conheça o PicPay Card">
</button>
```

**O que o axe reporta:**

```
Rule: button-name
Impact: critical
Tags: wcag2a, wcag412, section508
Message: "Ensure buttons have discernible text"
Element: <button class="teaser__dot active">
Fix any of:
  - Element does not have inner text
  - aria-label attribute does not exist or is empty
  - aria-labelledby attribute does not exist, references elements that do not exist or references elements that are empty
  - Element has no title attribute
```

---

**`link-name` — Links sem texto discernível** (🔴 Critical)

```html
<!-- ❌ Encontrado no picpay.com -->
<!-- Seletor: div.card-carousel__item__action > a -->
<a href="https://blog.picpay.com/carteira-de-trabalho-digital/" 
   role="button" 
   data-gtm-name="CARTEIRA_DE_TRABALHO_DIGITAL">
</a>

<!-- ✅ Correção — texto visível (melhor para SEO e acessibilidade) -->
<a href="https://blog.picpay.com/carteira-de-trabalho-digital/">
  Ler artigo: Carteira de trabalho digital
</a>

<!-- ✅ Correção alternativa — aria-label quando o design exige link sem texto -->
<a href="https://blog.picpay.com/carteira-de-trabalho-digital/" 
   aria-label="Ler artigo sobre carteira de trabalho digital">
  <span class="card-carousel__arrow" aria-hidden="true">→</span>
</a>
```

---

**`image-alt` — Imagens sem `alt`** (🟠 Serious)

```html
<!-- ❌ Encontrado no picpay.com -->
<!-- Seletor: div.selector__item > img -->
<img src="https://publish-p153277-e1666050.adobeaemcloud.com/adobe/dynamicmedia/deli…" 
     loading="eager" decoding="async" width="200" height="250">

<!-- ✅ Correção para imagem informativa -->
<img src="https://publish-p153277-e1666050.adobeaemcloud.com/adobe/dynamicmedia/deli…" 
     alt="Cartão PicPay na variante verde com chip contactless"
     loading="eager" decoding="async" width="200" height="250">

<!-- ✅ Correção para imagem decorativa -->
<img src="..." alt="" role="presentation">
```

**Dica axe:** o axe classifica `image-alt` como "Serious" (não Critical) porque a imagem pode ser decorativa — mas sem `alt`, ele não tem como saber. Na dúvida, o axe reporta.

---

**`color-contrast` — Contraste insuficiente** (🟠 Serious)

O axe mostra a ratio exata e as cores foreground/background detectadas:

```html
<!-- ❌ Encontrado no picpay.com -->
<!-- Seletor: div.wrapped-teaser > div.anchor__wrap > a > h2 -->
<!-- axe reporta: foreground #999999, background #ffffff, ratio 2.85:1 (mínimo 4.5:1) -->
<h2 style="color: #999999;">Conheça nossos cartões</h2>

<!-- ✅ Correção — ratio 7:1 -->
<h2 style="color: #595959;">Conheça nossos cartões</h2>
```

```css
/* ❌ CSS do PicPay — ratio 2.85:1 */
.wrapped-teaser h2 {
  color: #999999;
}

/* ✅ Correção — ratio 7:1 com fundo branco */
.wrapped-teaser h2 {
  color: #595959;
}

/* ✅ Alternativa — usar variável do design system para manter consistência */
.wrapped-teaser h2 {
  color: var(--text-secondary, #595959); /* garante contraste mínimo AA */
}
```

---

**`label-content-name-mismatch` — Label visível ≠ nome acessível** (🟠 Serious)

```html
<!-- ❌ Encontrado no picpay.com -->
<!-- Seletor: div.footer__qrcode > a.footer__qrcode__button -->
<!-- axe reporta: visible text "Baixe o app" não está contido no aria-label -->
<a href="https://marketing.marketing.picpay.com/HPHx/tql2vlv1" 
   role="button"
   aria-label="Baixar o aplicativo PicPay">
  Baixe o app  <!-- "Baixe" ≠ "Baixar" — não faz match -->
</a>

<!-- ✅ Correção — aria-label CONTÉM o texto visível -->
<a href="https://marketing.marketing.picpay.com/HPHx/tql2vlv1" 
   role="button"
   aria-label="Baixe o app PicPay para Android ou iOS">
  Baixe o app
</a>
```

**Regra do axe:** o `aria-label` deve **conter** o texto visível como substring. Isso garante que usuários de controle por voz consigam ativar o botão falando o que veem na tela.

---

**`target-size` — Alvos de toque < 24×24px** (🟠 Serious)

```html
<!-- ❌ Encontrado no picpay.com -->
<!-- Seletor: div.cardgrid__dots > button.cardgrid__dot -->
<!-- axe reporta: "Target size of 8x8px is less than minimum of 24x24px" -->
<button type="button" class="cardgrid__dot" aria-label="Ir para o slide 1" 
        style="display: block;">
</button>
```

```css
/* ❌ CSS do PicPay — 8×8px, um terço do mínimo WCAG */
.cardgrid__dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  padding: 0;
}

/* ✅ Correção — manter visual, expandir área de toque com padding */
.cardgrid__dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  padding: 8px;        /* área total: 24×24px */
  background-clip: content-box; /* visual continua 8×8px */
}

/* ✅ Alternativa — usar pseudo-elemento (sem impacto no layout) */
.cardgrid__dot {
  width: 8px;
  height: 8px;
  position: relative;
}
.cardgrid__dot::before {
  content: '';
  position: absolute;
  inset: -8px; /* expande para 24×24px sem afetar o visual */
}
```

---

**`heading-order` — Hierarquia fora de ordem** (🟡 Moderate)

```html
<!-- ❌ Encontrado no picpay.com -->
<!-- Seletor: div.cardgrid__item__text > h4 -->
<h1>PicPay — Tecnologia que simplifica</h1>
  <h2>Conheça nossos cartões</h2>
    <h4>PicPay Card</h4>          <!-- ⚠️ pula h3 -->

<!-- ✅ Correção -->
<h1>PicPay — Tecnologia que simplifica</h1>
  <h2>Conheça nossos cartões</h2>
    <h3>PicPay Card</h3>           <!-- hierarquia sequencial -->
```

---

**`region` — Conteúdo fora de landmarks** (🟡 Moderate — Best Practice)

O axe detecta que parte do conteúdo do PicPay está fora de `<main>`, `<nav>`, `<header>` ou `<footer>` — tornando a navegação por landmarks impossível.

```html
<!-- ❌ Encontrado no picpay.com — layout sem landmarks semânticos -->
<body>
  <div class="header">...</div>        <!-- deveria ser <header> -->
  <div class="content">...</div>        <!-- deveria ser <main> -->
  <div class="footer">...</div>         <!-- deveria ser <footer> -->
</body>

<!-- ✅ Correção — usar HTML semântico -->
<body>
  <header class="header">...</header>
  <main class="content">...</main>
  <footer class="footer">...</footer>
</body>
```

**Por que landmarks importam:** leitores de tela oferecem atalho para pular direto para `<main>` (NVDA: tecla `D`). Sem `<main>`, o usuário precisa percorrer todos os links do header antes de chegar ao conteúdo.

---

### O que o axe detecta que o Lighthouse não detecta

| Rule ID | axe | Lighthouse |
|---|---|---|
| `region` | ✅ Conteúdo fora de landmarks | ❌ Não verifica |
| `landmark-one-main` | ✅ Página sem `<main>` | ❌ Não verifica |
| `duplicate-id-aria` | ✅ IDs duplicados referenciados por ARIA | ⚠️ Parcial |
| `aria-allowed-attr` | ✅ Atributos ARIA inválidos para o role | ✅ Verifica também |

O axe-core roda **~90 regras**, o Lighthouse roda **~50** (subconjunto do axe-core). Por isso o axe é mais completo — ele inclui regras classificadas como "best practices" que o Lighthouse ignora.

---

## Contexto da auditoria

```
Ferramenta : axe DevTools — Web Accessibility Testing
Versão     : 4.91.0 (extensão Chrome)
Motor      : axe-core 4.10.2
Criador    : Deque Systems
Escopo     : Homepage picpay.com
Padrão     : WCAG 2.1 A/AA + Best Practices
```

> **Nota**: O axe tem a menor taxa de falso-positivo entre as ferramentas automatizadas — se ele reporta, é erro. Isso o torna ideal para CI/CD onde falsos-positivos geram fadiga de alerta.
