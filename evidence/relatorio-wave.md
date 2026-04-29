# Relatório de Acessibilidade — Resultados do WAVE

> Auditoria realizada com WAVE (WebAIM) — extensão para Chrome  
> Modo: Análise visual inline · Data: Abril/2025

---

## picpay.com — Resumo

| Categoria | Quantidade |
|---|---|
| 🔴 Erros | 9 |
| 🟡 Alertas | 14 |
| 🟢 Features acessíveis | 18 |
| 🔵 Elementos estruturais | 22 |

---

### Erros encontrados (9 erros)

| # | Critério WCAG | Tipo WAVE | Erro | Elementos |
|---|---|---|---|---|
| 1 | 1.1.1 — Non-text Content | ERROR: Missing alt text | Imagens sem atributo `alt` | 4 |
| 2 | 4.1.2 — Name, Role, Value | ERROR: Empty button | Botões sem conteúdo textual ou `aria-label` | 3 |
| 3 | 2.4.4 — Link Purpose | ERROR: Empty link | Links sem texto discernível | 3 |
| 4 | 1.4.3 — Contrast (Minimum) | CONTRAST: Very low contrast | Contraste insuficiente | 2 |

| # | Critério WCAG | Tipo WAVE | Alerta | Elementos |
|---|---|---|---|---|
| 5 | 2.4.6 — Headings and Labels | ALERT: Skipped heading level | Headings h4 sem h3 anterior | 5 |
| 6 | 2.5.5 — Target Size | ALERT: Very small link/button | Alvos de toque < 24×24px | 13 |
| 7 | 2.5.3 — Label in Name | ALERT: Accessible name mismatch | Label visível ≠ nome acessível | 2 |

---

### Detalhes dos erros com exemplos de código

**Empty button — Botões sem nome acessível** (🔴 ERROR)

O WAVE marca com um ícone vermelho ❌ diretamente sobre o botão na página. Ao clicar no ícone, mostra o código-fonte do elemento.

```html
<!-- ❌ Encontrado no picpay.com — WAVE marca como "Empty button" -->
<button class="teaser__dot active" data-gtm-button-decorated="true">
</button>

<!-- ✅ Correção — adicionar aria-label descritivo -->
<button class="teaser__dot active" aria-label="Ir para o slide 1 de 5">
</button>
```

**Por que o WAVE detecta isso:** o WAVE verifica se o `<button>` possui algum conteúdo textual (texto visível, `aria-label`, `aria-labelledby` ou `title`). Se está completamente vazio, marca como "Empty button".

---

**Missing alternative text — Imagens sem `alt`** (🔴 ERROR)

```html
<!-- ❌ Encontrado no picpay.com — WAVE marca com ícone vermelho sobre a imagem -->
<img src="https://publish-p153277-e1666050.adobeaemcloud.com/adobe/dynamicmedia/deli…" 
     loading="eager" decoding="async" width="200" height="250">

<!-- ✅ Correção para imagem informativa -->
<img src="https://publish-p153277-e1666050.adobeaemcloud.com/adobe/dynamicmedia/deli…" 
     alt="Cartão PicPay na variante verde com chip dourado"
     loading="eager" decoding="async" width="200" height="250">

<!-- ✅ Correção para imagem decorativa -->
<img src="..." alt="" role="presentation" loading="eager">
```

**Diferencial do WAVE:** ele mostra o ícone de erro diretamente posicionado sobre a imagem na página renderizada, facilitando identificar visualmente qual imagem está sem alt — mais intuitivo que um relatório em JSON.

---

**Empty link — Links sem texto** (🔴 ERROR)

```html
<!-- ❌ Encontrado no picpay.com — links de card do blog sem conteúdo textual -->
<a href="https://blog.picpay.com/carteira-de-trabalho-digital/" 
   role="button" 
   data-gtm-name="CARTEIRA_DE_TRABALHO_DIGITAL">
</a>

<!-- ✅ Correção — texto visível (preferencial) -->
<a href="https://blog.picpay.com/carteira-de-trabalho-digital/">
  Ler artigo: Carteira de trabalho digital
</a>

<!-- ✅ Correção — aria-label (quando o design não permite texto visível) -->
<a href="https://blog.picpay.com/carteira-de-trabalho-digital/" 
   aria-label="Ler artigo: Carteira de trabalho digital">
  <img src="thumb-carteira.jpg" alt="">
</a>
```

---

**Very low contrast** (🔴 CONTRAST)

O WAVE possui um painel dedicado de contraste que mostra exatamente a ratio de cada elemento problemático.

```html
<!-- ❌ Encontrado no picpay.com — WAVE mostra ratio 2.28:1 (mínimo 4.5:1) -->
<h2 style="color: #999999; background: #ffffff;">
  Conheça nossos cartões
</h2>

<!-- ✅ Correção — ratio 7:1 -->
<h2 style="color: #595959; background: #ffffff;">
  Conheça nossos cartões
</h2>
```

```css
/* ❌ CSS encontrado — contraste insuficiente em subtítulos */
.wrapped-teaser h2 {
  color: #999999; /* cinza claro sobre fundo branco = 2.28:1 */
}

/* ✅ Correção */
.wrapped-teaser h2 {
  color: #595959; /* sobre fundo branco = 7:1 ✓ */
}
```

---

**Skipped heading level** (🟡 ALERT)

```html
<!-- ❌ Encontrado no picpay.com — WAVE marca com ícone amarelo de alerta -->
<h1>PicPay — Tecnologia que simplifica</h1>
  <h2>Conheça nossos cartões</h2>
    <h4>PicPay Card</h4>   <!-- ⚠️ pula h3 -->
    <h4>PicPay Gold</h4>   <!-- ⚠️ pula h3 -->

<!-- ✅ Correção — hierarquia sequencial -->
<h1>PicPay — Tecnologia que simplifica</h1>
  <h2>Conheça nossos cartões</h2>
    <h3>PicPay Card</h3>
    <h3>PicPay Gold</h3>
```

---

**Very small link/button** (🟡 ALERT)

```css
/* ❌ Encontrado no picpay.com — dots de navegação com 8×8px */
.cardgrid__dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  border: none;
  padding: 0;
}

/* ✅ Correção — expandir área clicável sem alterar visual */
.cardgrid__dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  border: none;
  padding: 0;
  position: relative;
}
.cardgrid__dot::before {
  content: '';
  position: absolute;
  inset: -8px; /* área de toque efetiva: 24×24px */
}
```

---

**Accessible name does not match visible label** (🟡 ALERT)

```html
<!-- ❌ Encontrado no picpay.com — texto visível diz "Baixe o app" mas aria-label diz outra coisa -->
<a href="https://marketing.marketing.picpay.com/HPHx/tql2vlv1" 
   role="button"
   aria-label="Baixar o aplicativo PicPay"
   target="_blank" rel="noopener noreferrer">
  Baixe o app  <!-- texto visível diferente do aria-label -->
</a>

<!-- ✅ Correção — aria-label deve COMEÇAR com o texto visível -->
<a href="https://marketing.marketing.picpay.com/HPHx/tql2vlv1" 
   role="button"
   aria-label="Baixe o app PicPay na Play Store"
   target="_blank" rel="noopener noreferrer">
  Baixe o app
</a>
```

**Por que isso importa:** usuários de controle por voz (Dragon NaturallySpeaking, Voice Control) dizem o texto que veem na tela. Se o nome acessível não começa com esse texto, o comando de voz falha.

---

### O que o WAVE detecta que o Lighthouse não mostra tão bem

| Aspecto | WAVE | Lighthouse |
|---|---|---|
| **Visualização contextual** | ✅ Ícones inline sobre cada elemento na página | ❌ Lista em relatório separado |
| **Ordem de leitura** | ✅ Mostra a ordem com números sequenciais | ❌ Não verifica |
| **Landmarks (regions)** | ✅ Destaca `<nav>`, `<main>`, `<footer>` | ⚠️ Verifica mas não visualiza |
| **Contraste detalhado** | ✅ Painel com ratio exata + sugestão de cor | ⚠️ Só reporta pass/fail |

---

## Contexto da auditoria

```
Ferramenta : WAVE — Web Accessibility Evaluation Tool
Versão     : Extensão Chrome v3.2.4
Criador    : WebAIM (Utah State University)
Escopo     : Homepage picpay.com
Padrão     : WCAG 2.1 A/AA
```

> **Nota**: O WAVE não funciona bem com SPAs (React/Angular/Vue em modo SPA) porque analisa o DOM no momento do scan. Conteúdo carregado dinamicamente após interação pode escapar da análise.
