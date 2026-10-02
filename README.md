# regex-tester-with-explainer

Test a regular expression against sample text AND get a plain-English breakdown of what each token does. Single HTML file. Browser-only.

**Live demo:** https://0xelitesystem.github.io/regex-tester-with-explainer/

## Why this exists

Regex101 is great for testing but its explainer is dense. Most quick "test this regex" questions don't need a full debugger; they need a fast match-and-explain loop. This is that.

The explainer is the headline feature. It walks the pattern token by token (anchors, character classes, quantifiers, groups, escape sequences, alternation, lookarounds) and produces a numbered list of what each part does in plain English. Useful for:

- Learning regex without reading a full reference
- Documenting team-shared patterns ("here's the regex we use, here's what it does")
- Code review when the pattern is too gnarly to read at a glance
- Debugging your own regex when you forgot what you wrote yesterday

## Use

Open `index.html` in any browser. Or visit the hosted version at `https://0xelitesystem.github.io/regex-tester-with-explainer/` once GitHub Pages is enabled.

1. Type or paste a regex pattern.
2. Toggle flags by clicking: `g` (global), `i` (case-insensitive), `m` (multiline), `s` (dotall).
3. Type or paste sample text below.
4. Matches highlight in the test text in real time.
5. Below the matches, the pattern is broken down into a numbered explanation.

## What gets explained

The tokenizer recognizes:

- **Anchors**: `^`, `$`, `\b`, `\B`
- **Character classes**: `[abc]`, `[^abc]`, ranges like `[a-z]`
- **Built-in classes**: `\d`, `\D`, `\w`, `\W`, `\s`, `\S`, `\n`, `\r`, `\t`
- **Quantifiers**: `*`, `+`, `?`, `{n}`, `{n,}`, `{n,m}`, with lazy variants (`*?`, `+?`, etc.)
- **Groups**: capture `( )`, non-capture `(?: )`, named `(?<name> )`
- **Lookarounds**: lookahead `(?= )`, `(?! )`, lookbehind `(?<= )`, `(?<! )`
- **Alternation**: `|`
- **Wildcard**: `.`
- **Escape sequences**: `\\`, `\/`, `\.`, etc.
- **Literals**: coalesced into single tokens for cleaner output

## Engine

This tool uses the JavaScript regex engine (V8 / SpiderMonkey / WebKit, depending on browser). Patterns that work here may behave differently in:

- PCRE (Perl, PHP, most CLI tools like `grep -P`)
- Python's `re` module
- Go's `regexp`
- .NET regex
- Rust's `regex` crate

For example, JavaScript supports lookbehind in modern engines but not all browsers historically did. PCRE has features like recursive patterns that JS doesn't.

If you need to test a pattern for a specific engine, use that engine's playground.

## Tech

- Single HTML file
- ~750 lines including CSS and JS
- Vanilla JS, no frameworks, no dependencies, no build step
- Tokenizer is hand-written; not perfect but covers the common 95% of regex syntax
- Tested in current Chrome, Firefox, Safari
- Light and dark themes, OS preference honored
- WCAG AA contrast on both themes

## What it doesn't do

- Doesn't generate regex from natural language. That's a different problem (and an LLM problem, not a tokenizer problem).
- Doesn't show the regex tree visually. Some tools draw a railroad diagram; this one stays textual for printability and copy-paste.
- Doesn't benchmark performance. Patterns that match exponentially slow on adversarial input (catastrophic backtracking) won't be flagged here.
- Doesn't translate between regex flavors. JavaScript regex only.
- Doesn't store your patterns. Refreshing the page clears state.

## Privacy

Everything runs in your browser. The page makes no network requests and has no analytics. Your pattern and sample text stay on the page and are gone when you refresh. If you click the theme toggle, your light or dark choice is saved in your browser's localStorage under the key `theme`. Nothing else is stored.

## Run locally

```bash
git clone https://github.com/0xelitesystem/regex-tester-with-explainer
cd regex-tester-with-explainer
```

Open `index.html` in any browser. Or serve the folder with `python -m http.server 8000` and visit http://localhost:8000/.

## Build

No build step. The whole tool is one `index.html` file with its CSS and JavaScript inline.

## More

Part of a catalog of single-file browser tools and plain-language references, all MIT licensed and dependency-free: [0xelitesystem.github.io](https://0xelitesystem.github.io/). Built by [elitesystem.ai](https://elitesystem.ai).

## License

MIT. See [LICENSE](LICENSE).

## Related

- [prompt-diff](https://github.com/0xelitesystem/prompt-diff), compare two prompt versions with diff highlighting
- [og-image-generator](https://github.com/0xelitesystem/og-image-generator), generate Open Graph cards in your browser
- [single-file-saas-template](https://github.com/0xelitesystem/single-file-saas-template), ship a SaaS in one HTML file
- [prompt-templates](https://github.com/0xelitesystem/prompt-templates), production prompts targeting LLM failure modes
