# Attention, Worked Out

A single-page, dependency-free interactive explainer for the mechanism behind
[*Attention Is All You Need*](https://arxiv.org/abs/1706.03762) (Vaswani et al., arXiv:1706.03762, cs.CL).

**Live: https://pasekpasek.github.io/attention-worked-out/**

## What's in it

- **Where all this sits.** What the paper was doing in 2017, what a transformer is, and why the thing
  is under every language model. A shape strip showing the one fact that makes it click — 6×512 goes
  in, 6×512 comes out, twelve sub-layers in a row, and only attention lets one position see another.
  Then the architecture from Figure 1, with the blocks this page opens in amber and clickable straight
  to their section, the ones it does not in steel, and a toggle between the paper's translation model
  and the decoder-only stack that came after it.
- **Why the stack is six deep.** Composing the attention matrices across layers gives exactly how much
  of each original token survives in a vector after L layers, so that is what it computes. Depth is not
  what lets information arrive — full attention reaches everything at layer one — it is what lets it
  compound.
- **One head, hands on.** The n×n attention matrix over a short sentence. Click any row to see where
  that token looks. Four selectable heads, each a different affinity, because `qᵢ · kⱼ` is a bilinear
  form `xᵢ (W^Q W^Kᵀ) xⱼᵀ` — so a head *is* one matrix saying which kinds of token attend to which.
- **Why divide by √dₖ.** The paper's footnote says that for `q`, `k` with independent unit-variance
  components, `q · k` has variance `dₖ`. The page samples that live: drag `dₖ` and watch the measured
  variance track it, and the unscaled softmax collapse to one-hot somewhere around the paper's real
  head width of 64.
- **Many heads, one budget.** `h` attentions over narrower projections, `dₖ = d_model/h`, concatenated
  and projected. Not more computation — the same computation spent on more than one question.
- **The mask.** Illegal scores set to −∞ *before* the softmax, so rows renormalise over what remains
  instead of summing to less than one.
- **Attention cannot see order.** Shuffle the tokens and the output is identical to machine precision,
  because attention maps a set to a set. Turn on positional encoding and the same shuffle changes it.
  Plus the sinusoid heatmap and the relative-offset similarity curve.
- **What this buys.** Table 1 of the paper with your own `n` and `d` in it: complexity per layer,
  sequential operations, maximum path length.

## What is real and what is not

Every number is computed in your browser from the formulas in the paper — real softmax, real dot
products, real Gaussian sampling, real sinusoids, real complexity arithmetic. Nothing is animated
toward a predetermined answer.

The projection matrices are **toy values**, because the page ships no trained weights. They are chosen
so the arithmetic stays inspectable at `d_model = 4` rather than 512, and the page says so where it
matters. What a trained model puts in those matrices is a different question from what the mechanism
does with them, and this page is about the second one.

## Scope

The mechanism, not the model. The encoder–decoder stack, residual connections and layer
normalisation, the feed-forward sublayers, the warmup schedule, label smoothing, beam search and the
BLEU results are all in the paper and deliberately out of this page.

## Running it

One file, no build step.

```bash
python3 -m http.server 8000   # then open http://localhost:8000
```

## Credits

The paper is by Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones,
Aidan N. Gomez, Łukasz Kaiser and Illia Polosukhin.

Built by [Paweł Pasek](https://github.com/PasekPasek). Companion to
[spatiotemporal-composability](https://github.com/PasekPasek/spatiotemporal-composability), which
uses the same workshop-plate design.
