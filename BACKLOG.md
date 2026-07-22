# Backlog — Allan-Nava profile

Fonte unica dei task del profilo. Ogni item ha un `id` stabile e (se attivo) un'issue GitHub collegata.
Item `[backlog]` = idee/nice-to-have, non "next". Aggiornare qui **e** sull'issue quando lo stato cambia.

Legenda stato: 🟢 done · 🟡 pronto, bloccato su input · 🟠 idea · ⚪ in corso

## Feature bloccate su un secret (workflow già pronto)

| id | Feature | Stato | Serve | Issue |
|----|---------|-------|-------|-------|
| `waka` | WakaTime — coding stats settimanali (`waka.yml`) | 🟡 | secret `WAKATIME_API_KEY` | [#15](https://github.com/Allan-Nava/Allan-Nava/issues/15) |
| `metrics` | Metrics dashboard SVG su branch `assets-metrics` (`metrics.yml`) | 🟡 | PAT `METRICS_TOKEN` (`repo`+`read:user`) | [#16](https://github.com/Allan-Nava/Allan-Nava/issues/16) |
| `summary` | Profile summary cards su `assets-summary` (`profile-summary.yml`) | 🟡 | PAT `GH_TOKEN_SUMMARY` | [#24](https://github.com/Allan-Nava/Allan-Nava/issues/24) |
| `spotify` | Widget Spotify now-playing | 🟠 | istanza Vercel self-hosted + app Spotify | [#14](https://github.com/Allan-Nava/Allan-Nava/issues/14) |

Attivazione (waka/metrics/summary): aggiungi il secret in **Settings → Secrets → Actions**, poi *Actions → Run workflow*. Per `metrics` va poi aggiunto l'embed nel README (`raw.githubusercontent.com/Allan-Nava/Allan-Nava/assets-metrics/github-metrics.svg`).

## Idee / nice-to-have `[backlog]`

| id | Idea | Stato | Note |
|----|------|-------|------|
| `selfhost-stats` | Self-host `github-readme-stats` su Vercel per riavere Stats + Top-langs affidabili | 🟠 | alternativa a `metrics`; serve PAT + deploy Vercel |
| `pin-cards` | Tornare alle pin card grafiche nel Featured se si self-hosta github-readme-stats | 🟠 | ora è una tabella shields affidabile |
| `now-section` | Sezione "Now / currently working on" curata a mano | 🟠 | contenuto statico, zero dipendenze |

## Fatto 🟢

- Header animato (Typing SVG), tech stack `skillicons`, Streak, Activity Graph, Contribution Snake (light/dark), Featured Projects (shields), Latest Posts (feed `allan-nava.github.io`), Recent Activity.
- Infra: artefatti generati su branch dedicati (mai su master), workflow marker consolidato in `update-readme.yml`, automazione gestione issue (`stale.yml`, `issue-triage.yml`).
- Rimossi (servizi hosted down): Trophies (402), Stats/Top-langs pubblici (503), Dev Quote, dev.to posts, 3D calendar (snellimento).
