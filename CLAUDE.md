# CLAUDE.md — Allan-Nava

Repo **profilo GitHub** (`github.com/Allan-Nava/Allan-Nava`): il `README.md` è renderizzato come landing page del profilo su <https://github.com/Allan-Nava>. **Nessun codice applicativo, build o test**: il "prodotto" è il `README.md` renderizzato + le GitHub Actions che lo aggiornano in automatico.

## Regole di lavoro (SEMPRE)

- **MAI `git push`** — lo fa sempre l'utente. **MAI committare senza richiesta esplicita.** MAI `Co-Authored-By` nei commit.
- **Il branch di lavoro è `master`** (default branch del repo, dove committano tutte le Action). Il branch `main` è un fork stale da cancellare: lavorare lì significa che le fix non arrivano mai in produzione. Verificare sempre `git rev-parse --abbrev-ref HEAD` prima di toccare i workflow.
- **Commit gitmoji** — la history usa prefissi gitmoji-style (`:zap:`, `:robot:`, `:sparkles:`). Mantenere lo stile; i commit automatici delle Action usano `:robot:`.
- **Zone gestite dalle Action = intoccabili a mano** — il contenuto tra i marker HTML lo riscrivono i workflow. Mantenere i marker intatti, mai svuotarli/editarli a mano:
  - `<!--START_SECTION:activity-->` … `<!--END_SECTION:activity-->` (recent activity — `update-readme.yml`)
  - `<!-- BLOG-POST-LIST:START -->` … `<!-- BLOG-POST-LIST:END -->` (post da allan-nava.github.io — `update-readme.yml`)
  - `<!--START_SECTION:waka-->` … `<!--END_SECTION:waka-->` (coding stats — `waka.yml`, serve `WAKATIME_API_KEY`)
- **Artefatti generati stanno su branch dedicati, MAI su master** (così il push umano non va stale) — snake → `output`, summary → `assets-summary`. Il README li referenzia via URL raw. Non committare file generati su master.
- **`metrics.yml` si tematizza SOLO via `extras_css`** — l'input `config_theme` non esiste in `lowlighter/metrics`. I quadretti del calendario hanno `fill` hardcoded nell'SVG, quindi vanno presi con selettori d'attributo (`.day[fill="#216e39"]{fill:#10cf53}`): override su `:root` non hanno effetto. Verificabile in locale renderizzando l'SVG con Chrome headless.
- **Tema coerente SEMPRE** — accento verde `#10cf53` su sfondo nero `#050505`, testo bianco `#ffffff`. Ogni nuova stat card / badge / servizio va allineato alla palette.
- **Allineare tutto** — ogni modifica fattuale (nuovo workflow, nuova sezione, nuovo servizio) va propagata a `README.md`, ai marker, a `AGENTS.md` e a questo file.
- **Todo → `BACKLOG.md`** (fonte unica, item con `id` stabile + issue collegata). Non sparpagliare TODO. Item nice-to-have = 🟠 `[backlog]`, non proporli come "next". Aggiornare backlog **e** issue insieme quando lo stato cambia.
- **Issue** — gestione automatica via `stale.yml` (stale/close per inattività, esenti `backlog`/`blocked`/`needs-secret`/milestone) e `issue-triage.yml` (label da titolo). Le label custom (`needs-secret`, `blocked`, `backlog`, `stale`) devono esistere.
- **Segreti** — la maggior parte dei workflow usa il `GITHUB_TOKEN` built-in; fanno eccezione `waka.yml` (`WAKATIME_API_KEY`), `metrics.yml` (PAT **classic** `METRICS_TOKEN`, scope **solo `public_repo`**: il commit lo fa `committer_token` = `GITHUB_TOKEN`), `profile-summary.yml` (PAT `GH_TOKEN_SUMMARY`). Questi tre sono **gated**: secret esposto come `env` di job (il context `secrets` non è usabile in un `if` di job), step condizionati su `env.X != ''` → senza secret il job è no-op e resta verde, e si attiva da solo quando il secret viene aggiunto. Mantenere questo pattern per ogni nuovo workflow che dipende da un secret. Non introdurre altri secret senza segnalarlo. Mai token/credenziali in README o workflow.

## Layout

| Path | Ruolo |
|------|-------|
| `README.md` | La pagina profilo. La maggior parte delle modifiche tocca solo questo. |
| `.github/workflows/` | Le automazioni che rigenerano parti del README. |
| `*.png`, `*.jpeg`, `*.jpg`, `_cover.PNG` | Icone social e banner referenziati dal README (via `raw.githubusercontent.com`). |
| branch `output` / `assets-summary` / `assets-metrics` | Artefatti **generati** (snake / summary / metrics), non su master. |
| `.github/dependabot.yml` | **Unico** bot dipendenze (action raggruppate). `renovate.json` è disattivato (`"enabled": false`) per non avere PR duplicate. |

## Automazioni (non editare l'output generato a mano)

| Workflow | Scrive su | Trigger |
|----------|-----------|---------|
| `.github/workflows/update-readme.yml` | `README.md` (regioni `activity`, `BLOG-POST-LIST`) su master | ogni 6 h + push |
| `.github/workflows/waka.yml` | `README.md` (regione `waka`) su master — **serve** `WAKATIME_API_KEY`, altrimenti no-op verde | giornaliero |
| `.github/workflows/snake.yml` | branch `output` (SVG/GIF) | ogni 12 h + push |
| `.github/workflows/profile-summary.yml` | branch `assets-summary` (SVG) — **serve PAT** `GH_TOKEN_SUMMARY`, altrimenti no-op verde | giornaliero |
| `.github/workflows/metrics.yml` | branch `assets-metrics` (SVG) — **serve PAT** `METRICS_TOKEN`, altrimenti no-op verde | giornaliero |

## Trappole note / regole tecniche

- **Le Action che pushano richiedono** **Settings → Actions → General → Workflow permissions = "Read and write"**. Senza, falliscono in push.
- **Push umano che va stale**: committano su master solo `update-readme.yml` (ogni 6 h) e `waka.yml` (1×/giorno). Prima di pushare fare `git pull --rebase` (consigliato `git config pull.rebase true`). Snake/summary/metrics stanno su branch dedicati e NON toccano master.
- **L'immagine snake compare solo dopo il primo run** del workflow (branch `output` inizialmente assente). Lanciabili da *Actions → Run workflow*.
- **Immagini nel README** — repo images con URL raw completo (`https://raw.githubusercontent.com/Allan-Nava/Allan-Nava/master/<file>`) perché renderizzino sul profilo; file **generati** via URL raw dal loro branch (`.../Allan-Nava/output/<file>` per lo snake).
- **Badge** — usare `shields.io` stile `for-the-badge` per coerenza con la tech-stack row esistente.
- **Username per servizio** — `Allan-Nava` negli URL delle stat card; `allannava` su dev.to; `allan__nava` su X/Twitter. Non confonderli.
- **Validazione** — nessun test. Preview del markdown prima di consegnare; se tocchi un workflow valida lo YAML (`ruby -ryaml -e 'Dir[".github/workflows/*.yml"].each{|f| YAML.load_file(f)}'`).

## Puntatori

- Backlog operativo: `BACKLOG.md` · Milestone: `Profile v2` (#1) feature bloccate su secret, `Profile v3` (#2) content & polish.
- Profilo: <https://github.com/Allan-Nava> · Sito: <https://allan-nava.github.io/> · dev.to: <https://dev.to/allannava>
- Servizi/servizi-Action usati: `skillicons.dev` (tech stack), `img.shields.io` (badge/stelle), `github-readme-streak-stats.herokuapp.com`, `github-readme-activity-graph.vercel.app`, `readme-typing-svg.demolab.com`, `Platane/snk`, `gautamkrishnar/blog-post-workflow`, `athul/waka-readme`, `lowlighter/metrics`. (Trophies e `github-readme-stats.vercel.app` rimossi: istanze pubbliche down — 402/503.)
- L'utente è **Allan Nava**, DevOps Engineer @ HiWay Media (Milano). Repo di lavoro correlati: `devops_hiway`, `cnf-mng-hiway`.
