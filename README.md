# HSK Text Analyzer — 中文难度检测

**[→ Live Tool](https://ordinarymantrying.com/tools/hsk-text-analyzer.html)**

Paste any Chinese text. Every word is instantly color-coded by HSK level (1–6). See your comprehension percentage, get pinyin + English on hover, and export unknown words.

![HSK Text Analyzer](https://ordinarymantrying.com/tools/hsk-text-analyzer.html)

---

## What It Does

- **Color-coded segmentation** — greedy longest-match algorithm, no external library
- **HSK 1–6 coverage** — ~1,500-word dictionary across all 6 levels
- **Comprehension % bars** — cumulative per level (HSK1 only → HSK1+2 → ... → HSK1–6)
- **Hover / click popup** — pinyin + English meaning for every known word
- **Unknown word panel** — words beyond HSK 6 listed separately
- **One-click export** — download unknown words as `.txt`
- **4 sample texts** — HSK 2 story → HSK 3–4 WeChat article → HSK 5 AI news → HSK 6 cultural essay
- **Offline** — no server, no API, no login. Works after first load.

## Color Legend

| Color | Level | Vocabulary size |
|---|---|---|
| 🔵 Blue | HSK 1 | ~150 words |
| 🟢 Green | HSK 2 | ~300 words |
| 🟡 Yellow | HSK 3 | ~600 words |
| 🟠 Orange | HSK 4 | ~1,200 words |
| 🟥 Salmon | HSK 5 | ~2,500 words |
| 🔴 Red | HSK 6 | ~5,000 words |
| ⚫ Grey | Unknown | Beyond HSK 6 |

## Comprehension Guide

| Score | Interpretation |
|---|---|
| 95%+ | Comfortable reading |
| 85–95% | Readable with occasional dictionary |
| 70–85% | Challenging — look up frequently |
| < 70% | Advanced text — intensive study mode |

## Technical Notes

**Segmentation**: greedy longest-match, tries up to 6 characters, falls back to single-character.

```javascript
function segment(text) {
  const tokens = []; let i = 0;
  while (i < text.length) {
    const ch = text[i];
    if (!isCJK(ch)) { /* handle punctuation / newlines */ i++; continue; }
    let matched = false;
    for (let len = Math.min(6, text.length - i); len >= 1; len--) {
      const w = text.slice(i, i + len);
      if (D[w]) { tokens.push({ type:'word', text:w, lv:D[w][0], py:D[w][1], en:D[w][2] }); i += len; matched = true; break; }
    }
    if (!matched) { tokens.push({ type:'word', text:ch, lv:0, py:'', en:'' }); i++; }
  }
  return tokens;
}
```

**Dictionary format**: `D[word] = [level, pinyin, english]`, bulk-loaded with helper:

```javascript
const D = {};
function a(lv, arr) { arr.forEach(([w,p,e]) => { if (!D[w]) D[w] = [lv,p,e]; }); }
a(1, [["爱","ài","love"], ["八","bā","eight"], ...]);
```

## Use Cases

- **Students**: gauge whether a graded reader, news article, or movie subtitle is appropriate for your level
- **Teachers**: check if lesson materials match your students' vocabulary range
- **Self-study**: paste WeChat articles, song lyrics, TV transcripts to see where you stand

## Related Tools (same site)

- [Mandarin Flashcards](https://ordinarymantrying.com/tools/mandarin-flashcards.html) — HSK 1–3 spaced repetition · [GitHub](https://github.com/daligao/mandarin-flashcards)
- [Chinese Antonyms Game](https://ordinarymantrying.com/tools/chinese-antonyms.html) — 380 opposite-word pairs · [GitHub](https://github.com/daligao/chinese-antonyms-game)
- [Chinese Measure Words Game](https://ordinarymantrying.com/tools/chinese-measure-words.html) — 量词 sorting game · [GitHub](https://github.com/daligao/chinese-measure-words-game)
- [Chinese Writing Toolkit](https://ordinarymantrying.com/tools/chinese-writing-toolkit.html) — 11 types of applied writing

## License

MIT — free to use, fork, and modify.
