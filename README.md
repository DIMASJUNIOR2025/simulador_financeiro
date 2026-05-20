# 📊 Simulador de Prestações — PRICE vs SAC

> Comparação interativa entre sistemas de amortização PRICE e SAC com suporte a carência e evolução do saldo devedor.

![Badge](https://img.shields.io/badge/versão-1.0-4fffb0?style=flat-square&labelColor=1e1e2a)
![Badge](https://img.shields.io/badge/tipo-Vanilla%20HTML%2FJS-7b6ef6?style=flat-square&labelColor=1e1e2a)
![Badge](https://img.shields.io/badge/dependência-Chart.js%204.4-4fffb0?style=flat-square&labelColor=1e1e2a)
![Badge](https://img.shields.io/badge/idioma-Português%20BR-7b6ef6?style=flat-square&labelColor=1e1e2a)

---

## 📋 Índice

- [Visão Geral](#visão-geral)
- [Funcionalidades](#funcionalidades)
- [Demonstração](#demonstração)
- [Tecnologias](#tecnologias)
- [Como Usar](#como-usar)
- [Parâmetros de Entrada](#parâmetros-de-entrada)
- [Painéis de Resultado](#painéis-de-resultado)
- [Lógica Financeira](#lógica-financeira)
- [Estrutura do Código](#estrutura-do-código)
- [Customização](#customização)
- [Limitações Conhecidas](#limitações-conhecidas)

---

## Visão Geral

O **Simulador de Prestações** é uma aplicação web de página única (`index.html`) que permite comparar, lado a lado, os dois principais sistemas de amortização de empréstimos e financiamentos utilizados no Brasil:

| Sistema | Característica Principal |
|---------|--------------------------|
| **PRICE** (Francês) | Parcelas fixas durante todo o prazo |
| **SAC** (Constante) | Amortização fixa, parcelas decrescentes |

A ferramenta também suporta **período de carência**, onde os juros são capitalizados sem pagamento de parcelas, afetando o saldo devedor inicial antes do início da amortização.

---

## Funcionalidades

- **Cálculo PRICE** — prestação fixa com base no capital, taxa e prazo
- **Cálculo SAC** — amortização constante com parcelas decrescentes
- **Carência configurável** — de 0 a 24 meses, com juros compostos capitalizados
- **Taxa mensal ou anual** — conversão automática por equivalência composta
- **Resumo financeiro** — totais, juros pagos e economia do SAC sobre o PRICE
- **Gráfico de evolução** — linha do saldo devedor ao longo do tempo, com zona de carência destacada
- **Tabela de amortização** — parcela a parcela, com valores PRICE e SAC, juros e amortização
- **Interface responsiva** — adapta-se a dispositivos móveis e desktops
- **Tema escuro** — design moderno com paleta de cores personalizável via CSS variables

---

## Demonstração

```
Capital: R$ 10.000,00
Taxa:    1,5% ao mês
Prazo:   24 meses
Carência: 0 meses
```

| Métrica | PRICE | SAC |
|---------|-------|-----|
| 1ª Parcela | R$ 499,24 | R$ 566,67 |
| Última Parcela | R$ 499,24 | R$ 422,92 |
| Total Pago | R$ 11.981,76 | R$ 11.875,00 |
| Juros Totais | R$ 1.981,76 | R$ 1.875,00 |

> O SAC gera uma **economia de R$ 106,76** neste cenário, mas exige uma entrada maior.

---

## Tecnologias

| Recurso | Detalhes |
|---------|----------|
| **HTML5** | Estrutura semântica, arquivo único sem build |
| **CSS3** | Custom Properties (variáveis), Grid, Flexbox, animações |
| **JavaScript (ES6+)** | Vanilla JS, sem frameworks |
| **[Chart.js 4.4.1](https://www.chartjs.org/)** | Gráfico de linha com plugin customizado de zona de carência |
| **[Google Fonts](https://fonts.google.com/)** | Space Mono (mono) + DM Sans (body) |

> **Zero dependências de build.** Basta abrir o arquivo `index.html` no navegador.

---

## Como Usar

### Execução Local

```bash
# Clone ou baixe o arquivo
git clone https://github.com/seu-usuario/simulador-financeiro.git

# Abra diretamente no navegador (sem servidor necessário)
open index.html
# ou arraste o arquivo para uma aba do navegador
```

### Via Servidor Local (opcional)

```bash
# Python 3
python -m http.server 8080

# Node.js (npx)
npx serve .

# Acesse em: http://localhost:8080
```

> **Nota:** O Chart.js e as fontes são carregados via CDN. É necessária conexão com a internet para o primeiro acesso; após isso, o navegador armazena em cache.

---

## Parâmetros de Entrada

| Campo | Tipo | Padrão | Descrição |
|-------|------|--------|-----------|
| **Capital (R$)** | Número | `10.000` | Valor principal do empréstimo ou financiamento |
| **Tipo de Taxa** | Select | `Mensal` | Define se a taxa informada é mensal ou anual |
| **Taxa (%)** | Número | `1,5` | Percentual de juros no período selecionado |
| **Prazo (meses)** | Inteiro | `24` | Número de parcelas a pagar (1–360) |
| **Carência (meses)** | Slider | `0` | Período sem pagamento de parcelas (0–24) |

### Observações sobre os Parâmetros

**Taxa Anual → Mensal**
A conversão utiliza equivalência composta (não proporcional):

```
taxa_mensal = (1 + taxa_anual)^(1/12) - 1
```

**Carência**
Durante a carência, os juros são capitalizados sobre o capital original. O saldo devedor no início da amortização será:

```
saldo_pós_carência = capital × (1 + taxa_mensal)^meses_carência
```

---

## Painéis de Resultado

A área de resultados é dividida em três abas:

### 1. Resumo

Exibe quatro cartões com as métricas principais:

- **PRICE — Prestação**: valor fixo de cada parcela
- **SAC — 1ª Prestação**: maior parcela (decai ao longo do tempo)
- **Total PRICE**: soma de todas as parcelas + juros totais pagos
- **Total SAC**: soma de todas as parcelas + juros totais pagos

E uma tabela de detalhes com:
- Taxa mensal aplicada (com 4 casas decimais)
- Carência configurada
- Saldo devedor após carência
- Economia gerada pelo SAC em relação ao PRICE

### 2. Evolução

Gráfico de linha (Chart.js) mostrando a **curva de crescimento do saldo devedor** ao longo de todo o período (carência + amortização).

- Linha verde (`#4fffb0`) representa o saldo devedor crescendo durante a carência
- Zona roxa (`rgba(123,110,246,0.1)`) destaca visualmente o período de carência
- Tooltip interativo com valores em reais ao passar o mouse

> ⚠️ O gráfico atual exibe apenas a curva de capitalização (saldo crescendo). A curva de amortização (saldo caindo) pode ser adicionada como melhoria futura.

### 3. Amortização

Tabela completa com todas as parcelas lado a lado:

| Coluna | Descrição |
|--------|-----------|
| `#` | Número da parcela |
| `PRICE` | Valor fixo da prestação PRICE |
| `SAC` | Valor da prestação SAC naquele mês |
| `Juros (SAC)` | Componente de juros da parcela SAC |
| `Amort. (SAC)` | Componente de amortização (constante) |

A tabela possui rolagem vertical (`max-height: 340px`) para não quebrar o layout com prazos longos.

---

## Lógica Financeira

### Tabela PRICE (Sistema Francês)

Parcela constante calculada pela fórmula de anuidade:

```
PMT = PV × [i × (1+i)^n] / [(1+i)^n - 1]
```

Onde:
- `PV` = saldo devedor pós-carência
- `i` = taxa mensal
- `n` = número de parcelas

Caso a taxa seja zero, a parcela é simplesmente `PV / n`.

### Tabela SAC (Sistema de Amortização Constante)

```
amortização = PV / n
juros_i     = saldo_i × taxa_mensal
parcela_i   = amortização + juros_i
saldo_i+1   = saldo_i - amortização
```

A parcela **diminui** a cada mês porque os juros incidem sobre um saldo cada vez menor, enquanto a amortização permanece constante.

### Comparativo de Juros Totais

```
juros_PRICE = total_PRICE - capital_original
juros_SAC   = total_SAC   - capital_original
economia    = juros_PRICE - juros_SAC
```

O SAC **sempre gera menos juros totais** que o PRICE para os mesmos parâmetros, pois amortiza o saldo devedor mais rapidamente no início.

---

## Estrutura do Código

```
index.html
├── <head>
│   ├── Google Fonts (Space Mono + DM Sans)
│   ├── Chart.js 4.4.1 (CDN)
│   └── <style> — ~350 linhas de CSS com design system completo
│
└── <body>
    ├── .container
    │   ├── <header> — título e subtítulo
    │   └── .layout (CSS Grid: 320px | 1fr)
    │       ├── .card — Painel de Entrada (parâmetros)
    │       │   ├── #capital
    │       │   ├── #taxaTipo
    │       │   ├── #taxa
    │       │   ├── #meses
    │       │   ├── #carencia (range slider)
    │       │   └── button.btn → simular()
    │       └── .card — Painel de Resultados (abas)
    │           ├── #tab-resumo
    │           ├── #tab-evolucao → <canvas id="grafico">
    │           └── #tab-amortizacao
    │
    └── <script>
        ├── Sync slider (evento input)
        ├── showTab() — alternância de abas
        ├── taxaMensalEquivalente() — conversão anual → mensal
        ├── jurosCompostos() — capitalização do saldo
        ├── tabelaPrice() — cálculo da prestação PRICE
        ├── calcularSAC() — total e primeira parcela SAC
        ├── fmt() — formatação monetária pt-BR
        ├── pct() — formatação percentual
        └── simular() — função principal (leitura → cálculo → render)
            ├── Atualiza #resumo-content
            ├── Atualiza #grafico (Chart.js + plugin carenciaZone)
            └── Atualiza #tabela-content
```

### Variáveis CSS (Design Tokens)

```css
:root {
  --bg:           #0f0f13   /* fundo da página           */
  --surface:      #17171f   /* cards principais           */
  --surface2:     #1e1e2a   /* inputs e cards secundários */
  --border:       #2a2a38   /* bordas e separadores       */
  --accent:       #4fffb0   /* verde principal (PRICE)    */
  --accent2:      #7b6ef6   /* roxo (SAC)                 */
  --text:         #e8e8f0   /* texto principal            */
  --text-muted:   #7070a0   /* texto secundário/labels    */
  --danger:       #ff6b81   /* erros (reservado)          */
}
```

---

## Customização

### Alterar Cores

Edite as variáveis no bloco `:root` no `<style>`:

```css
:root {
  --accent:  #00d4ff;   /* Troca verde por azul */
  --accent2: #ff6b35;   /* Troca roxo por laranja */
}
```

### Alterar Limite de Carência

O slider atualmente permite até 24 meses. Para alterar:

```html
<!-- Linha ~135 do HTML -->
<input type="range" id="carencia" min="0" max="60" value="0" step="1">
```

### Alterar Limite de Prazo

```html
<!-- Linha ~129 do HTML -->
<input type="number" id="meses" value="24" min="1" max="360" step="1">
```

### Adicionar Saldo Devedor Decrescente ao Gráfico

No bloco `simular()`, adicione um segundo dataset ao Chart.js após o existente:

```javascript
// Dentro de chartInstance = new Chart(ctx, { data: { datasets: [ ... ] } })
{
  label: 'PRICE — Saldo',
  data: /* array com saldos decrescentes mês a mês */,
  borderColor: '#4fffb0',
  borderDash: [4, 4],
  // ...
},
{
  label: 'SAC — Saldo',
  data: /* array com saldos SAC */,
  borderColor: '#7b6ef6',
  // ...
}
```

---

## Limitações Conhecidas

| Limitação | Descrição |
|-----------|-----------|
| **Gráfico simplificado** | O gráfico de evolução exibe apenas a capitalização durante a carência, não as curvas de amortização PRICE e SAC separadas |
| **Sem IOF/TAC** | Não considera custos acessórios como IOF, tarifa de abertura de crédito ou seguros |
| **Sem indexadores** | Não suporta taxas variáveis (ex: IPCA+, CDI+) |
| **Sem amortizações extras** | Não simula pagamentos antecipados ou amortizações parciais |
| **Carência máxima** | Limitada a 24 meses pelo slider (editável no HTML) |
| **Sem persistência** | Os dados não são salvos entre sessões (sem localStorage) |

---

## Licença

Distribuído sob a licença **MIT**. Sinta-se livre para usar, modificar e distribuir.

---

> Desenvolvido com HTML, CSS e JavaScript puro · Sem frameworks · Sem build tools
