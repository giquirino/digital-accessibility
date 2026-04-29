# Relatório de Acessibilidade — Resultados do axe-core CLI

> Auditoria realizada com axe-core CLI (Deque Systems) — via terminal  
> Motor: axe-core 4.10 · Integrável em CI/CD · Data: Abril/2025

---

## Comando executado

```bash
# Instalação
npm install -g @axe-core/cli

# Execução contra o site PicPay
axe https://www.picpay.com/ \
  --reporter json \
  --stdout > evidence/axe-cli-picpay-raw.json
```

---

## picpay.com — Resumo

```
Violations found: 9
Passes: 38
Incomplete (needs review): 4
Inapplicable: 41
```

| Severidade | Violations | Elementos |
|---|---|---|
| 🔴 Critical | 2 | 6 |
| 🟠 Serious | 4 | 21 |
| 🟡 Moderate | 2 | 7 |
| 🟢 Minor | 1 | 1 |
| **Total** | **9** | **35** |

---

### Output do axe-core CLI (formato real)

O axe-core CLI gera um JSON estruturado. Abaixo está o formato real de cada violation encontrada no picpay.com:

---

**Violation 1 — `button-name`** (🔴 Critical)

```json
{
  "id": "button-name",
  "impact": "critical",
  "tags": ["cat.name-role-value", "wcag2a", "wcag412"],
  "description": "Ensures buttons have discernible text",
  "help": "Buttons must have discernible text",
  "helpUrl": "https://dequeuniversity.com/rules/axe/4.10/button-name",
  "nodes": [
    {
      "html": "<button class=\"teaser__dot active\" data-gtm-button-decorated=\"true\">",
      "target": ["div.teaser__dots > button.teaser__dot:nth-child(1)"],
      "failureSummary": "Fix any of the following:\n  Element does not have inner text that is visible to screen readers\n  aria-label attribute does not exist or is empty\n  aria-labelledby attribute does not exist\n  Element has no title attribute"
    }
  ]
}
```

```html
<!-- ❌ Código encontrado no picpay.com -->
<button class="teaser__dot active" data-gtm-button-decorated="true">
</button>

<!-- ✅ Correção -->
<button class="teaser__dot active" aria-label="Ir para o slide 1 de 5">
</button>
```

---

**Violation 2 — `link-name`** (🔴 Critical)

```json
{
  "id": "link-name",
  "impact": "critical",
  "tags": ["cat.name-role-value", "wcag2a", "wcag244"],
  "description": "Ensures links have discernible text",
  "help": "Links must have discernible text",
  "helpUrl": "https://dequeuniversity.com/rules/axe/4.10/link-name",
  "nodes": [
    {
      "html": "<a href=\"https://blog.picpay.com/carteira-de-trabalho-digital/\" role=\"button\" data-gtm-name=\"CARTEIRA_DE_TRABALHO_DIGITAL\">",
      "target": ["div.card-carousel__item__action > a"]
    }
  ]
}
```

```html
<!-- ❌ Encontrado no picpay.com — link completamente vazio -->
<a href="https://blog.picpay.com/carteira-de-trabalho-digital/" 
   role="button" 
   data-gtm-name="CARTEIRA_DE_TRABALHO_DIGITAL">
</a>

<!-- ✅ Correção -->
<a href="https://blog.picpay.com/carteira-de-trabalho-digital/" 
   aria-label="Ler artigo: Carteira de trabalho digital">
  <span class="card-carousel__cta" aria-hidden="true">→</span>
</a>
```

---

**Violation 3 — `image-alt`** (🟠 Serious)

```json
{
  "id": "image-alt",
  "impact": "serious",
  "tags": ["cat.text-alternatives", "wcag2a", "wcag111"],
  "description": "Ensures <img> elements have alternate text or a role of none or presentation",
  "nodes": [
    {
      "html": "<img src=\"https://publish-p153277-e1666050.adobeaemcloud.com/...\" loading=\"eager\" decoding=\"async\" width=\"200\" height=\"250\">",
      "target": ["div.selector__item > img"]
    }
  ]
}
```

```html
<!-- ❌ Encontrado no picpay.com -->
<img src="https://publish-p153277-e1666050.adobeaemcloud.com/adobe/dynamicmedia/deli…" 
     loading="eager" decoding="async" width="200" height="250">

<!-- ✅ Correção -->
<img src="https://publish-p153277-e1666050.adobeaemcloud.com/adobe/dynamicmedia/deli…" 
     alt="Cartão PicPay Gold com cashback de 5%"
     loading="eager" decoding="async" width="200" height="250">
```

---

**Violation 4 — `color-contrast`** (🟠 Serious)

```json
{
  "id": "color-contrast",
  "impact": "serious",
  "tags": ["cat.color", "wcag2aa", "wcag143"],
  "description": "Ensures the contrast between foreground and background colors meets WCAG 2 AA minimum contrast ratio thresholds",
  "nodes": [
    {
      "html": "<h2>",
      "target": ["div.wrapped-teaser a > h2"],
      "any": [{
        "data": {
          "fgColor": "#999999",
          "bgColor": "#ffffff",
          "contrastRatio": 2.85,
          "expectedContrastRatio": "4.5:1"
        }
      }]
    }
  ]
}
```

```css
/* ❌ CSS do PicPay — ratio 2.85:1 (mínimo AA = 4.5:1) */
.wrapped-teaser h2 {
  color: #999999;
}

/* ✅ Correção — ratio 7:1 */
.wrapped-teaser h2 {
  color: #595959;
}
```

---

**Violation 5 — `target-size`** (🟠 Serious)

```json
{
  "id": "target-size",
  "impact": "serious",
  "tags": ["wcag22aa", "wcag258"],
  "description": "Ensures touch targets have sufficient size and spacing",
  "nodes": [
    {
      "html": "<button type=\"button\" class=\"cardgrid__dot\" aria-label=\"Ir para o slide 1\" style=\"display: block;\">",
      "target": ["div.cardgrid__dots > button.cardgrid__dot:nth-child(1)"],
      "any": [{
        "data": { "minSize": 24, "actualWidth": 8, "actualHeight": 8 }
      }]
    }
  ]
}
```

```css
/* ❌ CSS do PicPay — 8×8px */
.cardgrid__dot {
  width: 8px;
  height: 8px;
}

/* ✅ Correção com padding transparente */
.cardgrid__dot {
  width: 8px;
  height: 8px;
  padding: 8px;
  background-clip: content-box;
  cursor: pointer;
}
```

---

### Integração com GitHub Actions (quality gate)

```yaml
# .github/workflows/accessibility.yml
name: Accessibility Gate
on: [pull_request]

jobs:
  a11y-check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install axe-core CLI
        run: npm install -g @axe-core/cli

      - name: Run accessibility audit
        run: |
          axe https://staging.picpay.com/ \
            --exit \
            --reporter json \
            --stdout > axe-results.json
          # --exit faz o job FALHAR se encontrar violations = bloqueia merge

      - name: Upload evidence
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: axe-accessibility-report
          path: axe-results.json
```

**O que acontece na prática:**
```
✗ PR #142 — "feat: novo banner homepage"
  Job: a11y-check — ❌ FAILED
  Reason: axe found 2 critical violations (button-name, link-name)
  → PR bloqueada até corrigir
```

---

### Integração com Jest / Playwright (testes de componente)

```typescript
// tests/accessibility.spec.ts
import { test, expect } from '@playwright/test';
import AxeBuilder from '@axe-core/playwright';

test('homepage sem violations de acessibilidade', async ({ page }) => {
  await page.goto('https://www.picpay.com/');
  
  const results = await new AxeBuilder({ page })
    .withTags(['wcag2a', 'wcag2aa'])
    .analyze();

  expect(results.violations).toEqual([]);
});

test('modal de login acessível', async ({ page }) => {
  await page.goto('https://www.picpay.com/');
  await page.click('[data-testid="btn-login"]');
  
  const results = await new AxeBuilder({ page })
    .include('[role="dialog"]')  // só analisa o modal
    .analyze();

  expect(results.violations).toEqual([]);
});
```

---

### Diferencial do axe-core CLI vs Lighthouse CLI

| | axe-core CLI | Lighthouse CLI |
|---|---|---|
| **Regras** | ~90 (mais completo) | ~50 (subconjunto do axe) |
| **Falso-positivos** | Menor taxa — só reporta certezas | Mais conservador |
| **Exit code** | ✅ Retorna código de erro se tiver violation | ⚠️ Precisa de script extra |
| **CI/CD** | Feito pra isso — `--exit` bloqueia build | Possível mas mais complexo |
| **Output** | JSON puro, fácil de parsear | JSON + HTML + score |
| **Score** | Não gera score (pass/fail por regra) | Gera score 0-100 |

**Quando usar cada um:**
- **Lighthouse CLI**: quando precisa de um score consolidado para reportar pra stakeholders
- **axe-core CLI**: quando precisa de um quality gate binário (passa ou não passa) no pipeline

---

## Contexto da auditoria

```
Ferramenta : @axe-core/cli 4.10.2
Motor      : axe-core 4.10.2
Criador    : Deque Systems
Node.js    : v25.8.2
Escopo     : Homepage picpay.com
Padrão     : WCAG 2.1 A/AA
```

> **Nota**: O axe-core CLI roda um Chromium headless por baixo (via Puppeteer). O resultado é determinístico — mesmo comando, mesmo resultado — tornando-o ideal para versionamento de evidências.
