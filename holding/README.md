# Holding: content for the blog and knowledge base projects

This folder is where the Writing, Knowledge and Glossary content is kept after being removed from the
portfolio (`vishal-portfolio`, bead `vishal-portfolio-9cm.16`). It seeds two future repos and is removed once
they exist:

- **blog** (`blog.biyani.xyz`): a proper blogging tool, seeded with `blog/content`.
- **kb** (`kb.biyani.xyz`): a knowledge base on Neon Postgres. The glossary is its first data.

Nothing here builds. Everything was copied from `vishal-portfolio@ceea576`; history for each file stays in that
repo (`git log -- <original path>`).

| Path | What it is | Came from |
|---|---|---|
| `blog/content/writing/` | 3 articles (Markdown with front matter) | `apps/web/content/writing/` |
| `blog/pipeline/` | The Markdown pipeline (remark/rehype, sanitise, Shiki) and its loader and fixtures | `apps/web/scripts/content/` |
| `blog/legacy-portfolio/` | The old site's blog list, post view, sidebar and in-browser editors, plus its sample data | `apps/portfolio/src/` |
| `kb/content/glossary.ts` | 68 payments and financial-services terms | `apps/web/content/glossary.ts` |
| `kb/content/knowledge/` | 3 domains (`domains.ts`) and 10 topics (Markdown) | `apps/web/content/knowledge/` |
| `kb/legacy-portfolio/` | The old site's knowledge base, glossary and 3-D Secure flow stepper, plus their original data files | `apps/portfolio/src/` |
| `shared/` | The zod schemas (`schema.ts`) and derived fields (`derive.ts`) the content was validated with | `apps/web/content/` |

Notes for the new projects:

- `shared/schema.ts` covers every portfolio content type; the article, topic, domain and glossary schemas are
  the relevant ones. For the Neon KB it is a good starting point for the table design.
- The 3-D Secure flow (`kb/content/knowledge/credit-cards-payments/3ds-flow.md`) was planned as an interactive
  step-by-step view; the old implementation is `kb/legacy-portfolio/components/knowledge/ThreeDSFlowStepper.jsx`.
- The old `apps/portfolio/docs-site` blog only held Docusaurus starter posts, so it was not copied.
