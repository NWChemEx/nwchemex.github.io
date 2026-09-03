---
title: "Conventions for Writing reST Documentation"
layout: single
permalink: /author/conventions/rst_conventions/
toc: true
toc_sticky: true
---

## Style

The style of reST pages follows Python's
[conventions](https://devguide.python.org/documenting/#documenting-python).
In particular:

- Follow the usual practice of at most 80 characters per line
- Indentation in reST is 3 spaces **NOT** 4

  - 3 spaces is natural for reST as this aligns with directives

## Sections

Python's reST
[conventions](https://devguide.python.org/documenting/#sections)
for denoting sections are:

- `#` with overline should be used for parts
- `*` with overline is for chapters
- `=` for sections
- `-` for subsections
- `^` for subsubsections
- `"` for paragraphs

The exact distinction between "parts", "chapters", *etc*. seems to be based
largely on how nested in the documentation a file is. For example, "parts" would
be topics in the top-level table of contents. "Chapters" would then be topics
in the table of contents linked to by the top-level table of contents, etc.

The definitions of parts, chapters, etc. are a bit annoying as they require
changing under/overlines in a potentially large number of documentation files if
any refactoring occurs. To avoid this, the NWX project adopts the convention
that definitions of parts, chapters, etc. are file specific. In other words,
the first title in a particular reST file should be considered the title of a
part; the first title within that part is considered a chapter, *etc.*.

## Citations and References

Pages that cite external literature should use the
[`sphinxcontrib-bibtex`](https://sphinxcontrib-bibtex.readthedocs.io/)
extension rather than docutils' native `.. [Key]` citation-list syntax,
since BibTeX is the field's standard bibliography format:

- Add `sphinxcontrib-bibtex` to the project's `docs/requirements.txt` and
  `"sphinxcontrib.bibtex"` to the `extensions` list in `docs/source/conf.py`,
  along with a `bibtex_bibfiles` setting pointing at the project's `.bib`
  file(s).
- Store references in a `docs/source/references.bib` file, one BibTeX entry
  per reference.
- Cite inline with the `:cite:` role, *e.g.* `` :cite:`whitten1973` `` (or
  `:cite:t:` for a textual citation such as "Weigend *et al.*"). BibTeX keys
  are lowercase `firstauthorsurname` + publication year (*e.g.* `whitten1973`),
  and unique across the file(s) listed in `bibtex_bibfiles`.
- Place a `References` subsection (`-` underline, per this file's section
  conventions) at the end of the page, containing a `.. bibliography::`
  directive.