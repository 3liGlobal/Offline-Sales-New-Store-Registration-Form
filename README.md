# Offline-Sales-New-Store-Registration-Form

> 3LI Global — AIR Global integration estate. Generated 2026-08-31 by the US-Infrastructure audit. Facts below are drawn from this repo's code; anything not directly evidenced is marked _unverified_.

## Connected Resource
- **Azure resource:** none — static HTML/CSS/JS page (no `.github/workflows`, no Azure config in the repo)
- **Deploy trigger:** _unverified_ — no CI/CD workflow is committed; hosting is external to this repo
- **Talks to:**
  - **Zoho Forms** — the form `POST`s (multipart) to `https://forms.zohopublic.com/airglobal/form/OfflineSales1/formperma/G82fh0Wip0qT5icphiEsg5oXkSK6WeisMJLADk9rso0/htmlRecords/submit`, feeding the AIR Global (`airglobal`) Zoho tenant / Zoho CRM
  - CDN assets: `intl-tel-input` and `font-awesome` from `cdnjs.cloudflare.com`
  - Links out to `https://business.hookah.com/content/terms-and-conditions` and `.../privacy-policy-business`

## What It Does
A new-store registration form used by offline-sales field agents. An agent (or a store) submits store/registration details including a phone number (international `intl-tel-input`) and country/state selection, and the record is captured into AIR Global's Zoho Forms/CRM (`OfflineSales1`).

## Why It Exists
It is the store-onboarding capture form embedded inside the offline-sales agent dashboard: the dashboard iframe wrapper passes the logged-in agent's context (`agentId`, `source`, `team`) into the form via URL query parameters, and those values are written into hidden Zoho fields so each new store is attributed to the agent/team that registered it. It sits alongside the KSA/USA offline-sales apps in this estate; the repo was cloned from the upstream `AlFakher2019/CRM-ZOHO-USA-Hookah-B2B-Registration-Form-Prod` (Hookah.com B2B / USA store registration), so it is the store-registration front end for that offline-sales flow.

## How It Works
1. `index.html` (titled "Hookah Registration Form") is a Zoho Forms HTML export; Zoho-generated field `name` attributes must be preserved or values submit empty.
2. On `DOMContentLoaded`, a script reads `agentId`, `source`, and `team` from `window.location.search` and injects them into hidden Zoho fields (`SingleLine9`, `SingleLine3`, `SingleLine10` respectively — an in-code comment warns these Zoho-auto-numbered names must be kept in sync if the form changes).
3. `js/countries-states.js` provides an ISO-3166 alpha-2 → alpha-3 conversion table used with the country/state and international phone widgets.
4. `js/validation.js` validates on submit, then the browser `POST`s to the Zoho `formperma` submit URL; Zoho records the entry (and can push to Zoho CRM per the form→CRM integration).
5. Hidden fields `zf_referrer_name`, `zf_redirect_url`, `zc_gad` are present but empty.
- **Operator note:** the query-param → hidden-field mapping depends on Zoho's auto-numbered `SingleLine*` names; if the Zoho form is edited and re-exported, re-verify that `source`/`agentId`/`team` still map to the correct fields.

---
_Environment:_ Production (upstream repo name carries a `-Prod` suffix; this repo has no suffix — inferred Production)
_Runtime:_ static HTML/CSS/JS (intl-tel-input + font-awesome via cdnjs; no server component)
