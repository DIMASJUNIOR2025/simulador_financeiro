# 💰 Simulador Financeiro — PRICE vs SAC

> Simulador de prestações de crédito em HTML5 puro, sem dependências de servidor ou frameworks externos. Compara os sistemas de amortização **PRICE** e **SAC** com suporte a carência, taxa mensal/anual e evolução gráfica do saldo devedor.

---

## 📋 Índice

- [Visão Geral](#visão-geral)
- [Funcionalidades](#funcionalidades)
- [Como Usar](#como-usar)
- [Cálculos Implementados](#cálculos-implementados)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Tecnologias](#tecnologias)
- [Parâmetros de Entrada](#parâmetros-de-entrada)
- [Abas de Resultado](#abas-de-resultado)
- [Origem do Projeto](#origem-do-projeto)

---

## Visão Geral

O **Simulador Financeiro PRICE vs SAC** é uma aplicação web desenvolvida em HTML5, CSS3 e JavaScript puro. Foi convertida a partir de um notebook Python (Jupyter + ipywidgets) para rodar diretamente no navegador, sem necessidade de instalação, servidor ou runtime Python.

A interface adota um tema escuro com grid de fundo, tipografia monospace/sans-serif e paleta verde-neon, projetada para leitura confortável de dados financeiros.

---

## Funcionalidades

- **Comparação PRICE × SAC** — calcula e exibe lado a lado as prestações de cada sistema
- **Carência configurável** — de 0 a 24 meses, com juros capitalizados sobre o saldo
- **Taxa mensal ou anual** — conversão automática para taxa mensal equivalente quando necessário
- **Evolução gráfica** — linha do saldo devedor ao longo do tempo, com zona sombreada no período de carência
- **Tabela de amortização** — parcela a parcela, com colunas de juros e amortização (SAC)
- **Resumo executivo** — 4 cards com prestação, total pago e economia entre os sistemas
- **Interface responsiva** — adapta-se a telas menores (mobile e tablet)
- **Zero dependências de servidor** — abre diretamente no navegador via `File > Open`

---

## Como Usar

### Opção 1 — Abrir localmente

```bash
# Não requer instalação. Basta abrir o arquivo no navegador:
double-click simulador-financeiro.html
# ou
open simulador-financeiro.html       # macOS
xdg-open simulador-financeiro.html  # Linux
```

### Opção 2 — Servir com servidor local (recomendado para desenvolvimento)

```bash
# Python 3
python -m http.server 8080

# Node.js (npx)
npx serve .

# Acesse: http://localhost:8080/simulador-financeiro.html
```

### Opção 3 — Deploy estático

O arquivo único pode ser hospedado em qualquer serviço de páginas estáticas:

| Serviço | Comando / Instrução |
|---------|---------------------|
| GitHub Pages | Commit o `.html` na branch `gh-pages` |
| Netlify | Arraste o arquivo para o painel de deploy |
| Vercel | `vercel --prod` na pasta do arquivo |
| Cloudflare Pages | Upload direto no dashboard |

---

## Cálculos Implementados

### Taxa Mensal Equivalente

Converte taxa anual nominal em mensal pelo regime de juros compostos:

```
r_mensal = (1 + r_anual)^(1/12) − 1
```

### Capitalização na Carência

Durante o período de carência, os juros são incorporados ao saldo devedor:

```
Saldo_pós_carência = Capital × (1 + r)^carência
```

### Sistema PRICE (Prestação Fixa)

Prestação constante calculada sobre o saldo pós-carência:

```
PMT = PV × [ r × (1+r)^n ] / [ (1+r)^n − 1 ]
```

Onde:
- `PV` = saldo pós-carência
- `r` = taxa mensal
- `n` = número de parcelas

### Sistema SAC (Amortização Constante)

Amortização fixa, juros decrescentes a cada parcela:

```
Amortização = PV / n
Parcela_i   = (PV − (i−1) × Amortização) × r + Amortização
```

A primeira parcela é sempre a mais alta; a última é a mais baixa.

---

## Estrutura do Projeto

```
simulador-financeiro.html   # Arquivo único autocontido
│
├── <style>                 # CSS (variáveis, layout, dark theme)
│   ├── Variáveis CSS       # Paleta, fontes, raios, transições
│   ├── Layout Grid         # Painel lateral + área de resultados
│   ├── Componentes         # Cards, inputs, slider, botão, tabs
│   └── Tabela & Gráfico    # Estilos de dados
│
├── <body>                  # Estrutura HTML
│   ├── Header              # Título e subtítulo
│   ├── Painel de Entrada   # Formulário de parâmetros
│   └── Painel de Saída     # Abas: Resumo / Evolução / Amortização
│
└── <script>                # JavaScript puro
    ├── Funções financeiras # taxaMensalEquivalente, tabelaPrice, calcularSAC
    ├── Renderização        # Resumo HTML, gráfico Chart.js, tabela dinâmica
    └── Interações          # Slider, tabs, botão simular
```

---

## Tecnologias

| Tecnologia | Versão | Uso |
|------------|--------|-----|
| HTML5 | — | Estrutura e semântica |
| CSS3 | — | Layout, variáveis, animações |
| JavaScript (ES2020) | — | Lógica financeira e DOM |
| [Chart.js](https://www.chartjs.org/) | 4.4.1 | Gráfico de evolução do saldo |
| [Google Fonts](https://fonts.google.com/) | — | Space Mono + DM Sans |

Todas as dependências externas são carregadas via CDN. O arquivo funciona **offline** se as fontes e o Chart.js já estiverem em cache no navegador.

---

## Parâmetros de Entrada

| Campo | Tipo | Padrão | Descrição |
|-------|------|--------|-----------|
| **Capital (R$)** | Número | 10.000 | Valor principal do financiamento |
| **Tipo de Taxa** | Select | Mensal | Mensal ou Anual (converte automaticamente) |
| **Taxa (%)** | Número | 1,5% | Taxa de juros no período selecionado |
| **Prazo (meses)** | Inteiro | 24 | Número de parcelas a pagar |
| **Carência** | Slider 0–24 | 0 | Meses sem pagamento (juros capitalizados) |

---

## Abas de Resultado

### 🗂 Resumo

Quatro cards comparativos exibindo:
- Prestação PRICE (fixa) e SAC (primeira parcela)
- Total pago em cada sistema
- Taxa mensal efetivamente aplicada
- Saldo após o período de carência
- **Economia total** optando pelo SAC

### 📈 Evolução

Gráfico de linha mostrando a curva do saldo devedor ao longo dos meses. A região sombreada em roxo indica o período de carência.

### 📊 Amortização

Tabela completa com todas as parcelas, incluindo:
- Prestação PRICE (badge verde)
- Prestação SAC (badge roxo, decrescente)
- Juros da parcela (SAC)
- Amortização da parcela (SAC)

---

## Origem do Projeto

Este projeto foi convertido a partir de um notebook **Jupyter (Python)** que utilizava:

```python
import ipywidgets as widgets
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
```

A conversão substituiu cada componente Python pelo equivalente web:

| Python / Jupyter | HTML5 |
|-----------------|-------|
| `ipywidgets.FloatText` | `<input type="number">` |
| `ipywidgets.IntSlider` | `<input type="range">` |
| `ipywidgets.Dropdown` | `<select>` |
| `ipywidgets.Tab` | Tabs em CSS + JS puro |
| `matplotlib.pyplot` | Chart.js |
| `pandas.DataFrame` | `<table>` gerada dinamicamente |
| `IPython.display.HTML` | Template literals JS |

---

## Licença

Distribuído sob a licença **MIT**. Sinta-se livre para usar, modificar e redistribuir.

---

<p align="center">Feito com HTML5 puro · Sem framework · Sem build step</p>
