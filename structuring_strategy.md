# HW2 – Semantic Structuring Strategy

## 1. Document outline of `structure.html`

```
1. Comp 484                                   <h1>  (in <header role="banner">)
   1.1 Structuring Pages                      <h2>  (in <header role="banner">)
2. <article> (page content)
   2.1 Conveying meaning through structure     <h3>  (article > header)
   2.2 Sectioning elements                     <h3>  (section)
   2.3 Document outlines                       <h3>  (section)
        2.3.1 W3C Warning                      <h4>  (aside role="complementary")
   2.4 WAI-ARIA Roles                          <h3>  (section)
3. <footer role="contentinfo">
   3.1 CSUN contact info
```

See `images/document-outline.svg` for the diagram version of this outline, and
`images/tag-structure-diagram.svg` for a box diagram of how the tags nest.

## 2. How the structural/sectioning tags are used

- **`<header role="banner">`** – wraps the site banner: the `h1` site title
  ("Comp 484"), the `h2` page title, and the primary `<nav>`. It appears once
  per page, directly inside `<body>`.
- **`<nav role="navigation">`** – wraps the single `<ul>` of page links shared
  by every page. It lives inside the banner `<header>` since it's part of the
  site chrome, not the page content.
- **`<main role="main">`** – wraps the one block of content that is unique to
  each page. There is exactly one `<main>` per page.
- **`<article>`** – wraps the page's self-contained body copy. Every page
  content is technically re-usable/syndicatable on its own (it doesn't depend
  on the rest of the page to make sense), so `article` is appropriate rather
  than a plain `section`.
- **`<header>` (nested, no role)** – used a second time *inside* `article` to
  group the article's own lead-in heading/paragraph, separate from the page
  banner header. Nested headers don't get `role="banner"` — only the one
  banner header for the whole document gets that role.
- **`<section>`** – used to break the article into thematic chunks (e.g.
  "Sectioning elements", "Document outlines", "WAI-ARIA Roles" on
  `structure.html`; "History", "HTML timeline" on `index.html`). Each section
  starts with its own heading, which is what promotes it to a new outline
  node.
- **`<aside role="complementary">`** – used for content that is related to,
  but not part of, the main flow of a section — the "W3C Warning" callout in
  `structure.html` and the "Web Design Resources" list in `next.html`. Both
  are supplementary rather than essential to reading the surrounding text.
- **`<footer role="contentinfo">`** – wraps the site-wide contact info at the
  bottom of every page.

## 3. Strategy applied consistently across the site

Every page (`index.html`, `syntax.html`, `structure.html`, `links.html`,
`reference.html`, `next.html`) follows the same skeleton so the site has one
predictable structure:

```html
<body>
  <header role="banner">
    <h1>Comp 484</h1>
    <h2>Page title</h2>
    <nav role="navigation"><ul>...</ul></nav>
  </header>
  <main role="main">
    <article>
      <header><!-- intro --></header>
      <section><!-- topic 1 --></section>
      <section><!-- topic 2 --></section>
      <!-- optional <aside role="complementary"> for supplementary content -->
    </article>
  </main>
  <footer role="contentinfo">...</footer>
</body>
```

## 4. ARIA roles applied

| Element              | Role                | Why |
|----------------------|---------------------|-----|
| outer `<header>`     | `banner`            | Identifies the site-wide masthead region |
| `<nav>`               | `navigation`        | Identifies the primary site navigation landmark |
| `<main>`              | `main`              | Identifies the page's main content landmark |
| outer `<footer>`     | `contentinfo`       | Identifies the site-wide footer/contact landmark |
| `<aside>`             | `complementary`     | Identifies supplementary content related to the main content |

Nested `<header>`/`<section>`/`<article>` elements that are *not* top-level
landmarks are left without a role, since HTML5's implicit ARIA mapping
already gives sectioning content the correct semantics once it's nested
inside a landmark, and adding redundant roles would just add noise for
assistive technology.
