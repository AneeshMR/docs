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
    openapi.yaml                           # OpenAPI 3.1 spec — source of truth
                                            # for the auto-generated, interactive
                                            # "Try it" API reference pages
```

## Publishing

1. Push this repo to GitHub.
2. Connect the repo at [dashboard.mintlify.com](https://dashboard.mintlify.com)
   — every push to `main` auto-deploys.
3. Add the custom domain `docs.ridergy.com` in the Mintlify dashboard and
   create the CNAME record it provides in DNS.

## Updating the API reference

`api-reference/energy-management/openapi.yaml` is the source of truth for the
Energy Management API reference pages. When endpoints change in
`Product/lambda/energy_management_api/`, update this spec to match — validate
with:

```bash
npx @redocly/cli lint api-reference/energy-management/openapi.yaml
```
