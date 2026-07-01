---
name: research-paper-data
description: Use Huanjing Studio Research API for scholarly paper search, DOI/arXiv/PMID/PMCID resolution, legal open-access PDF discovery, and paper reference relationship lookup. Use when users ask to find papers, inspect paper metadata, get paper PDFs, resolve scholarly identifiers, or build lightweight paper context for research writing. For /research/* requests, always use SIM_STUDIO_RESEARCH_API_BASE_URL, or strip trailing /v1 from SIM_STUDIO_API_BASE_URL; never append /research/* to the OpenAI-compatible /v1 model API base.
---

# Research Paper Data

Use this skill to give the agent a small, reliable data interface for research
writing. Prefer Huanjing Studio's server-side Research API before constructing
upstream provider URLs by hand.

## API Contract

Use `$env:SIM_STUDIO_RESEARCH_API_BASE_URL` and `$env:SIM_STUDIO_API_KEY` on
Windows PowerShell, or the equivalent environment variables in the current
shell. Send the Sim Studio API key as a bearer token. Do not ask the user for
Crossref, OpenAlex, CORE, Unpaywall, PubMed, or other provider keys.

`SIM_STUDIO_RESEARCH_API_BASE_URL` is the Console/root API origin, not the
OpenAI-compatible model base. Do not append `/research/*` to
`SIM_STUDIO_API_BASE_URL` when that value ends in `/v1`.

Available endpoints:

- `POST /research/papers/search` with `{ "query": "...", "limit": 8 }`
- `POST /research/papers/resolve` with `{ "id": "10.xxxx/..." }`
- `POST /research/papers/open-access` with `{ "id": "arXiv:2401.00001" }`
- `POST /research/papers/references` with `{ "id": "10.xxxx/..." }`

Example:

```powershell
$researchBase = $env:SIM_STUDIO_RESEARCH_API_BASE_URL
if (-not $researchBase -and $env:SIM_STUDIO_API_BASE_URL) {
  $researchBase = $env:SIM_STUDIO_API_BASE_URL -replace "/v1/?$", ""
}
$body = @{ query = "large language models scientific discovery"; limit = 8 } | ConvertTo-Json
Invoke-RestMethod `
  -Method Post `
  -Uri "$researchBase/research/papers/search" `
  -Headers @{ Authorization = "Bearer $env:SIM_STUDIO_API_KEY" } `
  -ContentType "application/json" `
  -Body $body
```

## Routing

- Use `search` for natural-language paper discovery.
- Use `resolve` when the user provides a DOI, arXiv ID, PMID, PMCID, paper URL,
  or bibliographic-looking identifier.
- Use `open-access` when the user asks for a legal PDF or full text location.
- Use `references` for DOI-based reference relationships and citation-context
  exploration.

## Response Handling

Treat returned `papers` as lightweight conversation objects. Number them
stably in the response so the user can say "paper 2" or "the second one".
Include title, authors, year, DOI/arXiv/PMID identifiers, URL/PDF URL, and the
short reason the paper matters when useful.

Mention provider provenance or provider failures only when it affects trust,
coverage, or next steps. If the API returns partial results, continue with the
available papers and say which source failed or timed out.

## Boundaries

- Use only legal open-access/source-discovery paths. Do not use Sci-Hub,
  publisher paywall bypasses, or similar download routes.
- Do not call CORE, OpenAlex paid paths, or any provider requiring a secret
  directly from the desktop. Provider keys belong on the Huanjing Studio server.
- If the Research API is unavailable, fall back only to safe public no-key
  sources such as Crossref, arXiv, PubMed/Europe PMC, DataCite, and
  OpenCitations. Never fall back to direct CORE calls.
- For user-provided local PDFs, use the available PDF/document tools or skills;
  this skill is for discovery, metadata, and relationship lookup.
