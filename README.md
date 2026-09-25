<div align="center">

# The Compound Interest Register

### See what time does to money.

An interactive compound interest calculator with a live growth chart, every currency your browser supports, and the Rule of 72, all in a single HTML file.

![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)
![Chart.js](https://img.shields.io/badge/Chart.js-4.4.1-FF6384?logo=chartdotjs&logoColor=white)

</div>

---

## Table of contents

1. [The idea](#the-idea)
2. [What it does](#what-it-does)
3. [The maths](#the-maths)
4. [How it works](#how-it-works)
5. [Tech stack](#tech-stack)
6. [Project structure](#project-structure)
7. [Run it locally](#run-it-locally)
8. [Try these scenarios](#try-these-scenarios)
9. [Design notes](#design-notes)
10. [Limitations](#limitations)
11. [Roadmap](#roadmap)

---

## The idea

Albert Einstein may or may not have called compound interest the eighth wonder of the world, but the maths certainly behaves like a wonder: small differences in **rate**, **time** and **compounding frequency** produce enormous differences in outcome.

Numbers in a formula don't make that obvious. A chart that redraws as you drag a slider does. This calculator is built to make the *shape* of compounding visible: the slow start, the bend, the runaway curve.

Its visual style is deliberately that of a financial register or ledger: a serif masthead, calm colours, and generous spacing, so it reads like a document, not a widget.

## What it does

- Three sliders: **Principal** ($500 to $100,000), **Annual rate** (1% to 20%), **Duration** (1 to 50 years).
- Four **compounding frequencies**: annual, quarterly, monthly, daily.
- A **currency selector** populated from the browser's own list of supported currencies, with proper locale formatting for each.
- Three headline numbers: **Invested**, **Interest earned** (with its share of the total), and **Final balance** (with its multiplier).
- A **line chart** of total balance versus principal over time.
- A **growth-multiplier strip** showing the multiple of your money at years 10, 20, 30, 40 and 50.
- A **Rule of 72** line: how many years until your money doubles.
- Updates instantly on any input, with a subtle reveal animation as sections scroll into view.

## The maths

The core is the standard compound interest formula:

```text
A = P × (1 + r / n) ^ (n × t)
```

| Symbol | Meaning |
|---|---|
| **A** | Final amount |
| **P** | Principal (starting amount) |
| **r** | Annual interest rate, as a decimal |
| **n** | Compounding periods per year (1, 4, 12 or 365) |
| **t** | Time in years |

In the code that is a one-liner:

```js
function calc(P, r, Y, n) {
  return P * Math.pow(1 + r / n, n * Y);
}
```

Derived numbers:

- **Interest earned** = `A − P`
- **Share of total** = `interest ÷ A`
- **Multiplier** = `A ÷ P`
- **Doubling time (Rule of 72)** ≈ `72 ÷ rate%`

## How it works

1. `update()` reads the three sliders and the selected frequency.
2. It calls `calc()` for every year from 0 to the chosen duration, producing a `totals` series and a flat `principals` series.
3. Headline numbers are formatted and written into the page.
4. The Chart.js instance's datasets are replaced and redrawn with `chart.update('none')`, so the chart updates without animating on every slider tick, which keeps dragging smooth.
5. The growth strip recomputes the balance at each 10-year checkpoint (limited to your duration) and scales bar heights against the largest value.
6. Slider `input` events and the frequency buttons all call `update()`.

**Currency handling** uses only built-in browser APIs:

- `Intl.supportedValuesOf('currency')` builds the dropdown, with a fallback list for older browsers.
- `Intl.DisplayNames` gives each currency its readable name.
- `Intl.NumberFormat` formats every figure using a locale matched to the currency (for example EUR to `de-DE`, INR to `en-IN`), and uses compact notation (`$1.2M`) on the chart axis.

## Tech stack

| Layer | Choice | Why |
|---|---|---|
| Markup | HTML5 | One page, semantic sidebar and main layout |
| Styling | CSS3 | Ledger-inspired design, responsive layout |
| Logic | Vanilla JavaScript | The maths is a single formula |
| Charts | [Chart.js 4.4.1](https://www.chartjs.org/) via cdnjs | A small, well-tested line chart |
| Formatting | The `Intl` API | Currency and locale support with zero extra libraries |
| Type | Newsreader (Google Fonts) | An elegant serif for the financial-document feel |

## Project structure

```text
compound-interest-calculator/
├── index.html   # markup, styles and script
└── README.md
```

## Run it locally

```bash
git clone https://github.com/4pfanas/compound-interest-calculator.git
cd compound-interest-calculator
open index.html
```

An internet connection is needed for Chart.js (loaded from a CDN) and the web font.

## Try these scenarios

| Scenario | Settings | What to notice |
|---|---|---|
| **Start early** | $5,000 · 7% · 40 years · monthly | Most of the final balance is interest, not your money |
| **Frequency barely matters** | Same inputs, switch annual → daily | The difference is small compared with changing the rate |
| **Rate matters a lot** | $5,000 · 20 years · try 4% vs 12% | Double the rate, far more than double the outcome |
| **Time is the real lever** | $5,000 · 7% · try 10 vs 30 years | The curve bends late, then explodes |

## Design notes

- **Slider, not a form.** Instant feedback teaches the relationships; a "Calculate" button hides them.
- **Chart updates without animation** so the picture tracks your hand while you drag.
- **Locale-aware money.** A yen figure should not be formatted like a dollar figure.
- **Restrained palette:** a single blue for growth and a pale green for principal.

## Limitations

- Models a **single lump sum** with no ongoing monthly contributions.
- Rate is treated as constant, with **no inflation, tax or fees**.
- Results are illustrative and not financial advice.

## Roadmap

- [ ] Regular monthly or yearly contributions
- [ ] Inflation-adjusted ("real") balance line
- [ ] Compare two scenarios side by side
- [ ] Shareable link that encodes the current inputs
- [ ] Export the chart as an image or CSV

---

<div align="center">

Built by **[Anas Aslam](https://github.com/4pfanas)**

</div>
