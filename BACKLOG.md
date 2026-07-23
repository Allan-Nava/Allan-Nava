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

## Pipeline nuovi progetti (VS Code extensions & tool) `[backlog]`

Idee nate il 2026-07-23 ragionando sullo stack DevOps (Nomad/Consul/NATS/HAProxy/Ansible/OTT). Le "fatte" sono scaffoldate con lo standard (CLAUDE.md+AGENTS.md stile devops_hiway, BACKLOG, CI, test).

| id | Progetto | Tipo | Stato | Note |
|----|----------|------|-------|------|
| `avl` | [ansible-vars-lens](https://github.com/Allan-Nava/ansible-vars-lens) — valore effettivo delle variabili Ansible per host, precedenza+provenienza, hover | VS Code ext | 🟢 | v0.1 completa, backlog interno per v0.2 |
| `natl` | [nats-lens](https://github.com/Allan-Nava/nats-lens) — client NATS: context CLI, pub/sub/request, JetStream browser | VS Code ext | 🟢 | v0.1 completa, semantic-release attivo |
| `noml` | [nomad-lens](https://github.com/Allan-Nava/nomad-lens) — tree cluster/job/alloc, log streaming, **plan diff repo-vs-running**, incident bundle, snapshot | VS Code ext | 🟢 | v0.1 pronta, repo da creare |
| `consul-lens` | Consul Lens — servizi+health drill-down, KV editor con check-and-set, watch su prefissi, vista leader Patroni | VS Code ext | 🟠 | ~50% riusabile da nats/nomad-lens; `consul agent -dev` per i test |
| `haproxy-lens` | HAProxy Config Lens — grafo frontend→ACL→backend, go-to-definition `use_backend`, lint | VS Code ext | 🟠 | sul Marketplace esiste solo syntax highlighting |
| `checkfleet` | [checkfleet](https://github.com/Allan-Nava/checkfleet) — monitoring domain-aware: check pluggabili in un binario Go (v0.1: `certs` con inventory Ansible + `http`); **assorbe** le ex-idee nats-doctor, certs-radar, stream-probe, haproxy-drift come moduli (`CF-1..4` nel suo backlog) | Go CLI | 🟢 | v0.1 completa con test; moduli nats/haproxy/stream/patroni + Slack/Prometheus nel backlog interno |
| `backlog-action` | backlog-sync-action — GitHub Action pubblicata: BACKLOG.md (id stabili) → issue/milestone idempotenti | GitHub Action | 🟠 | estrarre dal workflow già scritto in nats-lens; quick win |

## Fatto 🟢

- Header animato (Typing SVG), tech stack `skillicons`, Streak, Activity Graph, Contribution Snake (light/dark), Featured Projects (shields), Latest Posts (feed `allan-nava.github.io`), Recent Activity.
- Infra: artefatti generati su branch dedicati (mai su master), workflow marker consolidato in `update-readme.yml`, automazione gestione issue (`stale.yml`, `issue-triage.yml`).
- Rimossi (servizi hosted down): Trophies (402), Stats/Top-langs pubblici (503), Dev Quote, dev.to posts, 3D calendar (snellimento).
