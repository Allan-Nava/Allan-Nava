# AGENTS.md — Allan-Nava

Repo **profilo GitHub** (`github.com/Allan-Nava/Allan-Nava`): il `README.md` viene renderizzato come landing page del profilo su <https://github.com/Allan-Nava>. **Nessun codice applicativo, build o test**: il "prodotto" è il `README.md` renderizzato + le GitHub Actions che lo tengono aggiornato in automatico.

Questo file definisce le regole operative per gli agent (Copilot, Claude, altri tool AI) quando lavorano in questo repository.

## Regole di lavoro (SEMPRE)

- **MAI `git push`**: lo fa sempre l'utente. **MAI committare senza che l'utente lo chieda esplicitamente.** MAI `Co-Authored-By` nei commit.
- **Commit gitmoji**: la history usa prefissi gitmoji-style (`:zap:`, `:robot:`, `:sparkles:`). Mantenere lo stile. I commit automatici delle Action usano `:robot:`.
- **Zone gestite dalle Action = intoccabili a mano**: il contenuto tra i marker HTML lo riscrivono i workflow. Mantenere i marker intatti, mai svuotarli/editarli a mano:
  - `<!--START_SECTION:activity-->` … `<!--END_SECTION:activity-->` (recent activity — `update-readme.yml`)
  - `<!-- BLOG-POST-LIST:START -->` … `<!-- BLOG-POST-LIST:END -->` (post da allan-nava.github.io — `update-readme.yml`)
  - `<!--START_SECTION:waka-->` … `<!--END_SECTION:waka-->` (coding stats — `waka.yml`, serve `WAKATIME_API_KEY`)
- **Artefatti generati stanno su branch dedicati, MAI su master** (così il push umano non va stale): snake → `output`, summary cards → `assets-summary`. Il README li referenzia via URL raw da quei branch. Non committare file generati su master.
- **Tema coerente SEMPRE**: accento verde `#10cf53` su sfondo nero `#050505`, testo bianco `#ffffff`. Ogni nuova stat card / badge / servizio va allineato a questa palette.
- **Allineare tutto**: ogni modifica fattuale (nuovo workflow, nuova sezione, nuovo servizio) va propagata a `README.md`, ai marker, a questo file e a `CLAUDE.md`.
- **Segreti**: tutti i workflow usano solo il `GITHUB_TOKEN` built-in. Non introdurre nuovi secret senza segnalarlo esplicitamente. Mai token/credenziali nel README o nei workflow.

## Layout

| Path | Ruolo |
|------|-------|
| `README.md` | La pagina profilo. La maggior parte delle modifiche tocca solo questo. |
| `.github/workflows/` | Le automazioni che rigenerano parti del README. |
| `*.png`, `*.jpeg`, `*.jpg`, `_cover.PNG` | Icone social e banner referenziati dal README (serviti via `raw.githubusercontent.com`). |
| branch `output` / `assets-summary` | Artefatti **generati** (snake / summary), non su master. |
| `renovate.json`, `.github/dependabot.yml` | Config automazione dipendenze. |

## Automazioni (non editare l'output generato a mano)

| Workflow | Scrive su | Trigger |
|----------|-----------|---------|
| `.github/workflows/update-readme.yml` | `README.md` (regioni `activity`, `BLOG-POST-LIST`) su master | ogni 6 h + push |
| `.github/workflows/waka.yml` | `README.md` (regione `waka`) su master — **serve** `WAKATIME_API_KEY` | giornaliero |
| `.github/workflows/snake.yml` | branch `output` (SVG/GIF) | ogni 12 h + push |
| `.github/workflows/profile-summary.yml` | branch `assets-summary` (SVG) — **serve PAT** `GH_TOKEN_SUMMARY` | giornaliero |
| `.github/workflows/metrics.yml` | branch `assets-metrics` (SVG) — **serve PAT** `METRICS_TOKEN` | giornaliero |

## Trappole note / regole tecniche

- **Le Action che pushano richiedono** **Settings → Actions → General → Workflow permissions = "Read and write"**. Senza, falliscono in push.
- **Push umano che va stale**: committano su master solo `update-readme.yml` (ogni 6 h) e `waka.yml` (1×/giorno). Prima di pushare fare `git pull --rebase` (consigliato `git config pull.rebase true`). Snake/summary/metrics stanno su branch dedicati e NON toccano master.
- **Segreti richiesti** (finché mancano, i rispettivi workflow falliscono ma il README non si rompe): `WAKATIME_API_KEY` (waka), `METRICS_TOKEN` PAT (metrics), `GH_TOKEN_SUMMARY` PAT (summary). Spotify richiede istanza Vercel self-hosted separata.
- **L'immagine snake compare solo dopo il primo run** del workflow (branch `output` inizialmente assente). Lanciabili da *Actions → Run workflow*.
- **Immagini nel README**: repo images con URL raw completo (`https://raw.githubusercontent.com/Allan-Nava/Allan-Nava/master/<file>`) perché renderizzino sul profilo; file **generati** via URL raw dal loro branch (`.../Allan-Nava/output/<file>` per lo snake).
- **Badge**: usare `shields.io` stile `for-the-badge` per coerenza con la tech-stack row esistente.
- **Username per servizio**: `Allan-Nava` negli URL delle stat card; `allannava` su dev.to; `allan__nava` su X/Twitter. Non confonderli.
- **Validazione**: nessun test. Prima di consegnare, preview del markdown; se tocchi un workflow valida lo YAML (`ruby -ryaml -e 'Dir[".github/workflows/*.yml"].each{|f| YAML.load_file(f)}'`).

## Puntatori

- Profilo: <https://github.com/Allan-Nava> · Sito: <https://allan-nava.github.io/> · dev.to: <https://dev.to/allannava>
- Servizi/servizi-Action usati: `skillicons.dev` (tech stack), `img.shields.io` (badge/stelle), `github-readme-streak-stats.herokuapp.com`, `github-readme-activity-graph.vercel.app`, `readme-typing-svg.demolab.com`, `Platane/snk`, `gautamkrishnar/blog-post-workflow`, `athul/waka-readme`, `lowlighter/metrics`. (Trophies e `github-readme-stats.vercel.app` rimossi: istanze pubbliche down — 402/503.)
- L'utente è **Allan Nava**, DevOps Engineer @ HiWay Media (Milano). Repo di lavoro correlati: `devops_hiway`, `cnf-mng-hiway`.
