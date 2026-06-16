# Why English artifacts + Portuguese conversation

## The premise

Keep CLAUDE.md, specs, and docs in English; keep prompts and conversation in Portuguese.
Artifacts sit in the context window all the time — that is where the gain is largest.

## Evidence

### 1. English instructions outperform in-language prompts

Ahia et al., EMNLP 2023 — *Do All Languages Cost the Same? Tokenization in the Era of
Commercial Language Models* — provide every task instruction in English, following
Ahuja et al. (2023), who show that on several multilingual benchmarks **English
instructions outperform in-language prompts**. The material an agent *follows*
(CLAUDE.md, specs, ADRs) is therefore best written in English.

### 2. Tokenization makes English cheaper; fragmentation lowers utility

Same paper:

- Languages vary by up to ~5x in the number of tokens needed to convey the same
  content. Latin-script languages (including English and Portuguese) are among the
  cheapest; non-Latin scripts (e.g. Telugu, Tibetan) the most expensive.
- Higher fragmentation correlates with **lower in-context-learning utility** and
  higher API cost.
- Implication: material that re-enters context on every turn (artifacts) should be in
  the most token-efficient, best-followed language — English. Ephemeral Portuguese chat
  costs little, because pt-BR is Latin-script and tokenizes cheaply, so keeping the
  conversation in Portuguese is nearly free.

### 3. Why tokenization behaves this way: subword units / BPE

Sennrich, Haddow & Birch, 2016 (arXiv:1508.07909) — *Neural Machine Translation of Rare
Words with Subword Units* — introduced **Byte-Pair Encoding (BPE)** for open-vocabulary
models: start from characters and iteratively merge the most frequent adjacent pair into
a new symbol. Consequences relevant here:

- Frequent strings (abundant in English-heavy training corpora) collapse into single
  tokens; rarer strings fragment into many subword units.
- This is the mechanism behind the cost/fragmentation disparity above: the tokenizer is
  trained on mostly English data, so English compresses best.

## The synthesis (the rule)

- **Artifacts = persistent + repeatedly re-read** -> English: better instruction-following
  plus cheaper tokens, paid back every turn the artifact is in context.
- **Conversation = ephemeral** -> Portuguese: natural for the user, negligible token cost.
- The decision axis is **lifespan in the context window**, not the immediate reader.

## Sources

- Ahia, Kumar, Gonen, Kasai, Mortensen, Smith, Tsvetkov (2023). *Do All Languages Cost
  the Same? Tokenization in the Era of Commercial Language Models.* EMNLP 2023 Main,
  pp. 9904-9923. https://aclanthology.org/2023.emnlp-main.614.pdf
- Sennrich, Haddow, Birch (2016). *Neural Machine Translation of Rare Words with Subword
  Units.* arXiv:1508.07909. https://arxiv.org/pdf/1508.07909
