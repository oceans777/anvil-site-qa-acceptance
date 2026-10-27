---
name: anvil-site-qa-acceptance
description: >-
  Use when accepting an Anvil hot-keyword Codes/Wiki preview or production gate,
  or after i18n/UI/content changes—run the PlayGuideLab site-level checklist
  (locale purity, noindex, evidence, links, mobile) before claiming Pass.
---
# Anvil site QA acceptance

Canonical checklist (org): `PlayGuideLab/anvilwiki-next` → `docs/anvil-site-qa-acceptance.md`  
Parent ops: `docs/ops-supervision-acceptance.md` §4.1 — homepage-only green is **not** Pass.

Before claiming **Cloudflare-ready / open-index**, also run [Anvil CF SEO preflight](sand-workflow:anvil-cf-seo-preflight) (OSS SEO/SU + this checklist).

## When

- Before any Anvil consumer `site-launch` Pass
- After large content / i18n / mobile UI changes
- When the user reports locale mix, thin inner pages, prev/next layout, or video-facade issues

## Steps

1. Confirm template is **Anvil** (not game-sites long-form DB). Preview env = `public-preview`.
2. Build with `ANVIL_ENVIRONMENT=public-preview` (pin `ANVIL_EVALUATION_TIME` when reproducibility matters).
3. Run available contract checks: `check-content`, `check-i18n`, `check-links`.
4. **Locale purity:** run `node scripts/check-locale-purity.mjs` (or equivalent covering title, description, quickAnswer, bodyMarkdown, FAQ, videos, **bodyBlocks**). EN article HTML must have **zero CJK** (proper nouns like Emberville excluded only if Latin). Non-default locales must use prefixed internal links.
5. **Trap:** editing `bodyMarkdown` without clearing stale `bodyBlocks` leaves the old language on the page—always regenerate or delete `bodyBlocks`.
6. **SEO fail-closed:** `robots.txt` Disallow:/ ; meta + `X-Robots-Tag` noindex on preview. Never open index without the user’s written approval.
7. **Content:** evidence grades only; stubs labeled; no invented codes/class tables; video = poster + small centered play (no giant Load video CTA).
8. **Internal links:** hub cards HTTP 200; Keep exploring ∩ prev/next targets = empty set; mobile `.aw-prev-next` spacing readable.
9. **Mobile / visual:** hero flush under header (no corner triangles); FAQ closed card; default dark for new visitors; hero art present; no broken images; fair-use attribution on promo art.
10. **Process:** branch + `gh pr create` (no force-push to main); Vercel/static deploy git author matches the allowed deployer; Pass1 product + Pass2 eng/SEO; fill the acceptance-card template from the checklist.
11. **Fail-closed:** any locale or noindex blocker → do not claim Pass; keep noindex; fix before re-asking the user.

## Out of scope (escalate)

Domain purchase, Cloudflare/hosting passwords, AdSense IDs, production index — escalate with ≤3 concrete options.
