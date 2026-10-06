# cv

My curriculum vitae, typeset in LaTeX and published as a single self-contained
page: <https://jonathanwoollett-light.github.io/cv/>

## Layout

| File                 | Role                                                             |
| -------------------- | ---------------------------------------------------------------- |
| `cv.tex`             | **The source of truth.** All content lives here.                 |
| `build/template.html`| Page chrome: the stylesheet, theme toggle and print button.      |
| `index.html`         | **Generated.** Committed because GitHub Pages serves it directly.|

`index.html` is generated from `cv.tex`. Do not edit it by hand; run the build.

## Building

Requires [pandoc](https://pandoc.org/installing.html) on your `PATH` and Node.

```sh
npm install
npm run build      # cv.tex + build/template.html -> index.html
```

`npm run build` runs pandoc, then formats the result with prettier. CI rebuilds
`index.html` and fails if it differs from the committed copy, so always run the
build after editing `cv.tex` and commit the result.

`cv.tex` is ordinary LaTeX, so it also typesets to a PDF:

```sh
npm run pdf        # -> cv.pdf
```

## How the LaTeX maps to HTML

pandoc expands the `\newcommand` macros in `cv.tex`, so each one is written to
produce good output on both paths:

- `\entry{url}{organisation}{dates}{role}` becomes an `<h3>` holding the
  organisation and, in an `<em>`, the dates. The `\hfill` right-aligns the dates
  in the PDF; pandoc drops it and the stylesheet right-aligns the `<em>` instead.
- `\project{url}{Title:}` / `\projectplain{Title:}` start a paragraph with a
  run-in bold title, set with a hanging indent like a bibliography entry. Any
  paragraph that opens in bold directly under a section gets the same indent.
- `\setlength`, `\vspace` and the widow and club penalties shape only the PDF;
  pandoc skips them, and the stylesheet spaces the website's paragraphs.
- `\begin{center}` becomes the contact block under the name.

`--section-divs` wraps each section and each entry in a `<section>`, which the
stylesheet uses to keep entries from splitting across printed pages.

Because pandoc emits real typography (curly quotes, en dashes, bullets), the
template declares `<meta charset="utf-8">`. Do not remove it.
