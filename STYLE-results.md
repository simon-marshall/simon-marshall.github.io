# Results pages — style guide

For building a page that presents findings from data, in the Simon C. Marshall
visual identity. `sweets.html` is the reference implementation; read it before
building a new one.

This extends the base identity spec (the "Design for this page" box on
`index.html`). Everything there — palette, type, layout, voice — still applies.
What follows is what a *results* page adds.

---

## 1. The identity, in brief

```
--rock   #17130E   page ground, deepest
--ledge  #201A13   raised surfaces, boxes, chips
--seam   #332618   1px rules, borders, dividers
--ember  #E8862A   accent: links, emphasis, the one thing being pointed at
--sand   #F0E2CC   primary text
--dust   #B39B7C   secondary text, captions, prose in figures
--inset  #14100C   sunken panels, chart grounds
--key    #7C6950   readout key column, de-emphasised values
```

Muted data fills, darkest to lightest: `#2B2118`, `#3A2D1E`, `#4A3826`.

Amber is for one thing at a time. On a results page that thing is almost always
the series the sentence above the chart is about. If two things are amber, one
is wrong.

```
Display  Fraunces 600, -0.015em, line-height 1.02, clamp(2.6rem,7vw,4.1rem)
Body     Archivo 400, 17px, line-height 1.65
Utility  JetBrains Mono 400/500, 10.5-12px, letter-spacing .08-.14em, uppercase
Figure headings are Fraunces 400 at 1.16rem — the same size as a paper title.
```

Measure 660px, centred, 24px side padding. Border-radius 2–3px only. No shadows.
No gradients except the hero scrim.

---

## 2. Page shape

In order:

1. **Photo band.** `min(42vh,340px)`, the identity photograph, same scrim as the
   main hero. Carries the page title in Fraunces and the page nav. Shorter than
   the 74vh hero on the top-level pages — this signals "sub-page" without
   needing a breadcrumb.
2. **Intro.** At most two `.lede` paragraphs in `--dust`. Say what was recorded,
   over what period, and how honestly. Admit the sloppiness here rather than
   burying it in the methodology note.
3. **Readout.** The headline numbers, immediately. A reader who leaves after
   fifteen seconds should still leave with the result.
4. **Numbered sections.** `01 / The habit`, `02 / Two results` — mono eyebrow in
   `--dust`, with an optional `.standfirst` under it framing the section.
5. **Closing section.** What it adds up to. Totals, cumulative curves, the one
   number that makes the whole thing concrete.
6. **Methodology disclosure.** A closed `<details>`. Never inline — it interrupts
   the argument, and the people who want it will open it.
7. **Back-link and footer.**

Sections are separated by a 1px `--seam` top border with 52px of padding above
and below.

> Watch the specificity trap: `.wrap` sets `padding:0 24px`, and a bare `section`
> selector loses to it, silently killing the vertical rhythm. Match on
> `section.wrap` (or don't put `.wrap` on the section) whenever the section
> carries the measure class.

---

## 3. The readout

Headline numbers as a `<dl>` of key/value rows.

```html
<dl class="readout">
  <div><dt>Period covered</dt><dd>19 Feb 2024 &ndash; 22 Aug 2026</dd></div>
  <div><dt>Recorded days</dt><dd>774 <s>84% of the span</s></dd></div>
  <div><dt>Estimated energy</dt><dd>355,112 kcal <s>459/day</s></dd></div>
</dl>
```

```css
.readout{background:var(--inset);border:1px solid var(--seam);border-radius:3px;
  padding:6px 0;margin:34px 0 0}
.readout div{display:flex;gap:16px;padding:7px 18px;
  font-family:'JetBrains Mono',monospace;font-size:12px}
.readout dt{flex:0 0 176px;color:var(--key);margin:0;letter-spacing:.06em;
  text-transform:uppercase;font-size:10.5px;align-self:center}
.readout dd{margin:0;color:var(--sand)}
.readout dd s{text-decoration:none;color:var(--key)}
@media (max-width:520px){
  .readout div{flex-direction:column;gap:2px}
  .readout dt{flex:none;align-self:start}
}
```

Rules:

- **Six to eight rows.** Past that it stops being a headline and becomes a table.
- `<s>` is repurposed as a secondary value — the same quantity in another unit, a
  percentage, a counterpart. The strikethrough is removed; only the `--key`
  colour survives. It carries the "and, for scale…" half of the number.
- Keys are fixed-width so values form a column. Uppercase mono, `--key`.
- Values are `--sand`. The number leads; the unit follows.
- `dt`/`dd` must sit inside a `<dl>` to be valid. Keep the `<div>` row wrapper —
  it is what the flex layout hangs on, and it is legal inside `<dl>`.
- **`align-self:center` centres vertically in the row layout, but the narrow
  query flips to `flex-direction:column`, which makes the cross axis horizontal
  and centres every label sideways.** Pin `align-self:start` in the query.

---

## 4. Figures

One claim per figure. The structure is always heading → prose → chart.

```html
<figure>
  <h2>A heavy day predicts a heavier one</h2>
  <p>The obvious hypothesis is compensation: overdo it, then ease off. The data
  says the opposite. After a day over 100 g, the next day averages 119 g — above
  the overall mean of 103 g. Bars show standard error.</p>
  <img src="data:image/png;base64,…" alt="…">
</figure>
```

```css
figure{margin:0 0 48px} figure:last-child{margin-bottom:0}
figure h2{font-family:'Fraunces',Georgia,serif;font-weight:400;font-size:1.16rem;
  margin:0 0 10px;color:var(--sand)}
figure p{color:var(--dust);margin:0 0 14px;font-size:16px}
figure p:last-of-type{margin-bottom:20px}
figure img{width:100%;height:auto;display:block;background:var(--inset);
  border:1px solid var(--seam);border-radius:2px}
```

**The heading states the finding, not the topic.** "A heavy day predicts a
heavier one", not "Day-to-day correlation". "Only the staples are sticky", not
"Correlation by category". If the heading could sit above any chart of that
data, it is not doing its job.

**Prose goes above the chart, not below.** Tell the reader what to look for
before they look. Then give the numbers that support it. Then, if the effect is
partial or later contradicted, say so in a second short paragraph — do not leave
the correction for a section the reader may not reach.

---

## 5. Charts

Charts are rendered as images on an `--inset` ground so they sit inside the page
rather than on top of it.

- **Ground** `#14100C`. No white plot area, ever.
- **Series** muted browns `#2B2118` / `#3A2D1E` / `#4A3826`, with **exactly one**
  in `--ember` — the series the heading is about. A chart where everything is
  amber points at nothing.
- **Labels** JetBrains Mono, `--dust`, 10–11px. Axis titles in the same, smaller.
- **Rules and ticks** `--seam`. Gridlines only where a value must be read off;
  otherwise omit them.
- **Value labels** at the end of bars in `--dust`, so the reader never has to
  measure against an axis.
- **Error bars are not optional** where sampling noise could explain the effect.
  State in the prose what they represent — standard error, bootstrap 95%, or
  otherwise — and say which uncertainties they *exclude*.
- **Show the null.** If the figure claims a correlation, plot the shuffled
  baseline beside it in a pale muted fill, and say what it is. A reader cannot
  judge 0.32 without knowing that noise sits at 0.07.
- Sequential data goes left to right in time. Categorical bars sort by value,
  descending, unless there is a natural order.

---

## 6. Disclosures

```css
details summary{font-family:'JetBrains Mono',monospace;font-size:12px;
  letter-spacing:.08em;text-transform:uppercase;color:var(--ember);
  cursor:pointer;list-style:none;padding:8px 0}
details summary::-webkit-details-marker{display:none}
details summary::before{content:"+ "}
details[open] summary::before{content:"\2013 "}
```

Use them for the methodology, for a proof sketch, for anything a careful reader
wants and a casual one does not. Do not use them to hide a result — if it
belongs to the argument, it goes in the argument.

A disclosure that merely restates what is already on the page should be deleted
or turned into a link.

The explainer box, for a caveat that must be seen:

```css
.explainer{background:var(--ledge);border:1px solid var(--seam);
  border-left:2px solid var(--ember);border-radius:2px;padding:20px 22px}
```

---

## 7. Honesty

This is the part that makes it a results page rather than a slide.

- **Quantify uncertainty in the sentence, not just the chart.** "±7% for explicit
  weights, ±20–30% for bare named items, ±35–45% for vague entries."
- **Label the unproven.** When a result survives one test but not scrutiny, say
  so plainly and in the same breath: *"Testing four periods weakens that
  considerably; call it suggestive rather than settled."*
- **Separate sampling noise from systematic error.** If the error bars cover only
  one, say which, and say what the other would do: *"they cover sampling noise
  only — the correlated 15 to 20% uncertainty on absolute values sits on top, and
  would move every bar in a panel together rather than independently."*
- **Say when the bars do not clear.** *"December and August clear their
  neighbours comfortably; most of the six smaller panels do not clear their own
  error bars at all."* A results page that never admits a null result is not
  reporting, it is advertising.
- **Disclose every repair to the data.** How many rows were fixed, on what
  grounds, and what was assumed. Put it in the closing disclosure, in full.
- **Exclusions are stated where they bite,** not only in the methodology: *"The
  opening period was heavy enough to invent a spring peak that later years do not
  show, so it is excluded."*

---

## 8. Voice

Plain, declarative, understated. Sentence case. British spelling. No exclamation
marks. Humour dry and rare, and never in a heading that carries a finding.

- Lead with what was surprising. "The obvious hypothesis is compensation. The
  data says the opposite."
- Never let an adjective do a number's job. Not "chocolate dominates" but
  "chocolate appears on 74% of recorded days".
- Concede in the same paragraph as the claim, not in a footnote.
- Explain a cause when you have one, and stop when you do not: *"Easter eggs, and
  then no more Easter eggs."*
- Do not bold body text for emphasis. Use `--ember` or a mono label.

---

## 9. Checklist

- [ ] Headline numbers visible without scrolling past the intro
- [ ] Every figure heading states a finding, not a topic
- [ ] Exactly one amber series per chart
- [ ] Error bars present, and their meaning stated in prose
- [ ] Null / shuffled baseline shown wherever a correlation is claimed
- [ ] Every weak result labelled weak, in the sentence that makes it
- [ ] Methodology and data repairs in a closing disclosure
- [ ] `section.wrap`, not `section`, so the 52px rhythm survives
- [ ] `align-self:start` on `.readout dt` in the narrow query
- [ ] `dt`/`dd` inside a `<dl>`
- [ ] Checked at 1100px and under 520px
- [ ] `prefers-reduced-motion` collapses delays to zero, keeping end states
