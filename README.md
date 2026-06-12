# RiDERgy Docs

Public API documentation site, built with [Mintlify](https://mintlify.com),
intended to be published at **docs.ridergy.com** — separate from the
`Product` (backend) and `dashboard` (frontend) repos so doc releases aren't
coupled to app deploy cycles.

## Local development

```bash
npm i -g mintlify
mintlify dev
```

This serves the site locally (default http://localhost:3000) with hot reload.

## Folder structure

```
docs.json                                  # Mintlify site config + navigation
index.mdx                                  # Landing page
authentication.mdx                         # API key auth guide
concepts/
  priority-model.mdx                       # Limit profile / priority concepts
  errors.mdx                               # Error codes reference
api-reference/
  energy-management/
    openapi.<lang>.yaml                    # OpenAPI 3.1 spec per language
                                            # (de/en/es/fr/it/zh/pt) — descriptions
                                            # translated, endpoints/schemas identical
<lang>/api-reference/energy-management/     # per-endpoint stub .mdx pages, each
  *.mdx                                     # pinned to its language's spec (this is
                                            # how localized API reference works —
                                            # the auto `openapi` group collides all
                                            # languages onto one route, so don't use it)
```

> **Internal details are intentionally excluded** from all spec/concept text:
> no cron-job/function names (e.g. emLimitProcessor), internal table names, or
> smart-charging/rebalancing mechanics. Document user-facing *behavior* only.

## Publishing

This site lives at **docs.ridergy.com**, deployed by Mintlify from the
GitHub-connected repo (`AneeshMR/docs`); the canonical repo is `Ridergy/docs`,
so changes are pushed to **both** remotes:

```bash
git push origin main      # Ridergy/docs (canonical)
git push mintlify main:main   # AneeshMR/docs (Mintlify-connected)
```

> **⚠️ Publishing is a MANUAL step.** Mintlify auto-deploy from the connected
> repo is unreliable — builds frequently stall/queue and do not pick up the
> push. After pushing, **publish manually from the Mintlify dashboard**
> (the Deploy button). Do **not** wait/poll for the live site to update on its
> own, and do not try to force a deploy with empty commits — it doesn't help.

One-time setup (already done): repo connected at
[app.mintlify.com](https://app.mintlify.com) with the Mintlify GitHub App, and
custom domain `docs.ridergy.com` added with a CNAME →
`cname.mintlify.builders`.

## Updating the API reference

The `api-reference/energy-management/openapi.<lang>.yaml` specs are the source
of truth for the Energy Management API reference pages. When endpoints change in
`Product/lambda/energy_management_api/`, update **all 7** specs to match (keep
endpoints/schemas/examples identical across languages; translate only
descriptions — and keep internal details out, see the note above). Validate
with:

```bash
npx @redocly/cli lint api-reference/energy-management/openapi.*.yaml
```
