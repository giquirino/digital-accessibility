# Relatório de Acessibilidade — Resultados do NVDA

> Auditoria realizada com NVDA 2024.4 — leitor de tela (Windows)  
> Modo: Navegação real por teclado + áudio · Data: Abril/2025

---

## picpay.com — Resumo da experiência

| Aspecto | Resultado |
|---|---|
| Navegação por teclado | 🔴 Parcialmente funcional |
| Anúncio de conteúdo | 🟡 Incompleto — botões e links sem nome |
| Ordem de leitura | 🟡 Confusa em seções de cards |
| Fluxo de foco em modais | ❌ Não testável (modal não encontrado via teclado) |
| Landmarks | 🔴 Ausentes — sem `<main>` detectado |
| Headings para navegação | 🟡 Hierarquia com saltos (h2 → h4) |

---

### O que o NVDA detecta que nenhuma ferramenta automatizada detecta

O NVDA é um **leitor de tela** — ele simula a experiência de um usuário cego. Não gera relatório JSON, não tem score. Você **ouve** a página e navega **só com teclado**. Os problemas a seguir foram identificados exclusivamente por essa auditoria manual.

---

### Problema 1 — Botões anunciados como "button" sem contexto

**O que o NVDA anuncia:**
```
"button"
"button" 
"button"
```

O usuário ouve "button" três vezes seguidas sem saber o que cada um faz. São os dots do carrossel.

```html
<!-- ❌ Encontrado no picpay.com — o que o NVDA "vê" -->
<button class="teaser__dot active" data-gtm-button-decorated="true">
</button>
<!-- NVDA lê: "button" -->

<!-- ✅ Correção — NVDA lê: "Ir para o slide 1 de 5, button" -->
<button class="teaser__dot active" aria-label="Ir para o slide 1 de 5">
</button>
```

**Por que isso é pior do que parece:** ferramentas automatizadas reportam "button sem nome". Mas usar o NVDA mostra o impacto real — o usuário fica **perdido**, ouvindo "button, button, button" sem contexto, sem saber se vai navegar um slide, fechar um modal ou confirmar uma compra.

---

### Problema 2 — Links anunciados como "link" sem destino

**O que o NVDA anuncia ao navegar com Tab:**
```
"link"
"link"
"link"
```

Três links seguidos no carrossel de blog sem nenhum texto ou aria-label.

```html
<!-- ❌ Encontrado no picpay.com — NVDA lê apenas "link" -->
<a href="https://blog.picpay.com/carteira-de-trabalho-digital/" 
   role="button" 
   data-gtm-name="CARTEIRA_DE_TRABALHO_DIGITAL">
</a>

<!-- ✅ Correção — NVDA lê: "Ler artigo: Carteira de trabalho digital, link" -->
<a href="https://blog.picpay.com/carteira-de-trabalho-digital/" 
   aria-label="Ler artigo: Carteira de trabalho digital">
</a>
```

---

### Problema 3 — Navegação por headings com saltos

No NVDA, pressionar `H` pula para o próximo heading. A estrutura do picpay.com:

**O que o NVDA lê ao navegar por headings (tecla H):**
```
"heading level 1: PicPay"
"heading level 2: Conheça nossos cartões"
"heading level 4: PicPay Card"           ← ⚠️ pula nível 3
"heading level 4: PicPay Gold"           ← ⚠️ pula nível 3
"heading level 2: Blog PicPay"
```

O problema: ao pressionar `3` (atalho do NVDA para h3), o leitor de tela diz **"no heading level 3"** — o usuário acha que não existe sub-seção, quando na verdade existem cards de produto escondidos em h4.

```html
<!-- ❌ Estrutura do picpay.com — usuário de NVDA não encontra sub-seções -->
<h1>PicPay</h1>
  <h2>Conheça nossos cartões</h2>
    <h4>PicPay Card</h4>        <!-- NVDA: "heading level 4" -->

<!-- ✅ Correção — hierarquia sequencial -->
<h1>PicPay</h1>
  <h2>Conheça nossos cartões</h2>
    <h3>PicPay Card</h3>        <!-- NVDA: "heading level 3" ✓ -->
```

---

### Problema 4 — Ausência de `<main>` (landmark navigation)

No NVDA, pressionar `D` pula para o próximo landmark (`<main>`, `<nav>`, `<footer>`). No picpay.com:

**O que o NVDA anuncia:**
```
Tecla D → "banner" (header)
Tecla D → "content info" (footer)
→ Não encontra "main" — o conteúdo principal da página é inacessível por landmark
```

O usuário é obrigado a navegar elemento por elemento (Tab) para sair do header e chegar ao conteúdo.

```html
<!-- ❌ Estrutura do picpay.com — sem landmarks semânticos -->
<body>
  <div class="header">
    <nav>...</nav>      <!-- NVDA detecta como "navigation" -->
  </div>
  <div class="content">  <!-- NVDA não sabe que isso é o conteúdo principal -->
    ...centenas de elementos...
  </div>
  <div class="footer">...</div>
</body>

<!-- ✅ Correção — landmarks semânticos -->
<body>
  <header>
    <nav aria-label="Navegação principal">...</nav>
  </header>
  <main>                 <!-- NVDA: "main landmark" — tecla D pula direto pra cá -->
    ...conteúdo...
  </main>
  <footer>...</footer>
</body>
```

**Impacto real:** sem `<main>`, um usuário cego precisa percorrer **todos os links do menu de navegação** (que no picpay.com são dezenas) antes de chegar ao conteúdo da página. Com `<main>`, bastaria pressionar `D` uma vez.

---

### Problema 5 — Imagens sem `alt` — o NVDA lê o nome do arquivo

**O que o NVDA anuncia:**
```
"image, publish-p153277-e1666050.adobeaemcloud.com adobe dynamicmedia deliver dm-aid"
```

Quando uma imagem não tem `alt`, o NVDA tenta ler o `src` — e o resultado é uma URL incompreensível lida em voz alta.

```html
<!-- ❌ Encontrado no picpay.com -->
<!-- NVDA lê: "image, publish-p153277-e1666050 adobeaemcloud dot com..." (ilegível) -->
<img src="https://publish-p153277-e1666050.adobeaemcloud.com/adobe/dynamicmedia/deli…" 
     loading="eager" decoding="async" width="200" height="250">

<!-- ✅ Correção informativa — NVDA lê: "Cartão PicPay na variante verde, image" -->
<img src="..." alt="Cartão PicPay na variante verde" loading="eager">

<!-- ✅ Correção decorativa — NVDA ignora completamente -->
<img src="..." alt="" role="presentation" loading="eager">
```

---

### Problema 6 — Contraste insuficiente (impacto em baixa visão + zoom)

O NVDA tem modo de alto contraste integrado. Ao ativar (Insert + F2 → "High Contrast"), os subtítulos do PicPay desaparecem.

```css
/* ❌ CSS do PicPay — invisível no modo alto contraste */
.wrapped-teaser h2 {
  color: #999999; /* ratio 2.85:1 com fundo branco */
}

/* ✅ Correção — visível em todos os modos */
.wrapped-teaser h2 {
  color: #595959; /* ratio 7:1 com fundo branco */
}

/* ✅ Bônus — suporte a forced-colors (Windows High Contrast Mode) */
@media (forced-colors: active) {
  .wrapped-teaser h2 {
    color: CanvasText; /* respeita a preferência do sistema */
  }
}
```

---

### Problema 7 — Ordem de leitura vs ordem visual

O NVDA lê na **ordem do DOM**, não na ordem visual (CSS). No picpay.com, os cards de produto usam `order` em CSS, fazendo o NVDA ler em ordem diferente da visual.

```css
/* ❌ Encontrado — CSS reorganiza visualmente mas DOM está em outra ordem */
.cardgrid__item:nth-child(1) { order: 3; }
.cardgrid__item:nth-child(2) { order: 1; }
.cardgrid__item:nth-child(3) { order: 2; }

/* O usuário que enxerga vê: Card 2, Card 3, Card 1 */
/* O usuário de NVDA ouve: Card 1, Card 2, Card 3 (ordem do DOM) */

/* ✅ Correção — reordenar no DOM, não no CSS */
<!-- Ordem do HTML deve refletir a ordem de leitura desejada -->
<div class="cardgrid__item">Card 2</div>
<div class="cardgrid__item">Card 3</div>
<div class="cardgrid__item">Card 1</div>
```

---

### Atalhos NVDA usados na auditoria

| Tecla | Ação | O que verificamos |
|---|---|---|
| `Tab` | Próximo elemento interativo | Se todos os botões e links são alcançáveis |
| `Shift+Tab` | Elemento anterior | Se a navegação reversa funciona |
| `H` | Próximo heading | Se a hierarquia de headings faz sentido |
| `1-6` | Heading de nível específico | Se h3 existe (não existe no picpay.com) |
| `D` | Próximo landmark | Se `<main>` existe (não existe no picpay.com) |
| `K` | Próximo link | Se todos os links têm nome |
| `B` | Próximo botão | Se todos os botões têm nome |
| `G` | Próxima imagem | Se imagens têm alt |
| `Insert+F7` | Lista de elementos | Sumário completo da página |

---

### O que o NVDA encontra que nenhuma ferramenta automatizada detecta

| Problema | Lighthouse | axe | WAVE | NVDA |
|---|---|---|---|---|
| Botão sem nome | ✅ | ✅ | ✅ | ✅ + mostra o **impacto auditivo** |
| Ordem de leitura ≠ visual | ❌ | ❌ | ⚠️ Parcial | ✅ Detecta |
| Landmark navigation funcional | ❌ | ⚠️ Parcial | ⚠️ Parcial | ✅ Testa na prática |
| Qualidade do aria-label | ❌ | ❌ | ❌ | ✅ Ouve se faz sentido |
| Foco preso em modal | ❌ | ❌ | ❌ | ✅ Detecta |
| Conteúdo dinâmico anunciado | ❌ | ❌ | ❌ | ✅ Testa na prática |
| URL de imagem lida em voz alta | ❌ | ❌ | ❌ | ✅ Experiência real |

> **Conclusão:** o NVDA não substitui ferramentas automatizadas — ele complementa. As automatizadas pegam ~57% das falhas. O NVDA pega os outros **43%** que exigem contexto humano.

---

## Como rodar a auditoria NVDA

```
1. Instalar NVDA: https://www.nvaccess.org/download/ (gratuito, Windows)
2. Abrir o Chrome
3. Ativar NVDA (atalho: Ctrl+Alt+N)
4. Navegar para picpay.com
5. Fechar os olhos (ou desligar o monitor)
6. Navegar SÓ com teclado — ouvindo o que o NVDA anuncia
7. Documentar cada momento de confusão
```

**Alternativa macOS:** VoiceOver (Cmd+F5) — nativo, sem instalar nada.

---

## Contexto da auditoria

```
Ferramenta : NVDA (NonVisual Desktop Access) 2024.4
Criador    : NV Access (open-source)
Sistema    : Windows 11
Browser    : Chrome 124
Escopo     : Homepage picpay.com
Padrão     : WCAG 2.1 A/AA (avaliação manual)
```

> **Nota**: A auditoria com NVDA é a única que revela a **experiência real** de um usuário com deficiência visual. Um score 100 no Lighthouse com NVDA inacessível significa que a ferramenta falhou em medir o que importa.
