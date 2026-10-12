# AGENTS.md — Allan-Nava

Repo **profilo GitHub** (`github.com/Allan-Nava/Allan-Nava`): il `README.md` viene renderizzato come landing page del profilo su <https://github.com/Allan-Nava>. **Nessun codice applicativo, build o test**: il "prodotto" è il `README.md` renderizzato + le GitHub Actions che lo tengono aggiornato in automatico.

Questo file definisce le regole operative per gli agent (Copilot, Claude, altri tool AI) quando lavorano in questo repository.

## Regole di lavoro (SEMPRE)

- **MAI `git push`**: lo fa sempre l'utente. **MAI committare senza che l'utente lo chieda esplicitamente.** MAI `Co-Authored-By` nei commit.
- **Commit gitmoji**: la history usa prefissi gitmoji-style (`:zap:`, `:robot:`, `:sparkles:`). Mantenere lo stile. I commit automatici delle Action usano `:robot:`.
- **Zone gestite dalle Action = intoccabili a mano**: il contenuto tra i marker HTML lo riscrivono i workflow. Mantenere i marker intatti, mai svuotarli/editarli a mano:
  - `<!-- BLOG-POST-LIST:START -->` … `<!-- BLOG-POST-LIST:END -->` (ultimi progetti dal feed `/projects/feed.xml` di allan-nava.github.io — `update-readme.yml`)
- **Profilo snello, pensato per chi recluta** (2026-10-13): presentazione, What I work on, 6 progetti in evidenza, tech stack, ultimi progetti. Streak, metrics, snake, recent activity e WakaTime tolti insieme ai loro workflow: non rimetterli senza richiesta. I branch `output`, `assets-summary`, `assets-metrics` restano inutilizzati, da cancellare solo se l'utente lo decide.
- **Tema coerente SEMPRE**: accento verde `#10cf53` su sfondo nero `#050505`, testo bianco `#ffffff`. Ogni nuova stat card / badge / servizio va allineato a questa palette.
- **Allineare tutto**: ogni modifica fattuale (nuovo workflow, nuova sezione, nuovo servizio) va propagata a `README.md`, ai marker, a questo file e a `CLAUDE.md`.
- **Todo → `BACKLOG.md`** (fonte unica, item con `id` stabile + issue collegata). Non sparpagliare TODO. Item nice-to-have = 🟠 `[backlog]`, non proporli come "next". Aggiornare backlog **e** issue insieme quando lo stato cambia.
- **Issue**: gestione automatica via `stale.yml` (stale/close per inattività, esenti `backlog`/`blocked`/`needs-secret`/milestone) e `issue-triage.yml` (label da titolo). Le label custom (`needs-secret`, `blocked`, `backlog`, `stale`) devono esistere.
- **Segreti**: tutti i workflow usano solo il `GITHUB_TOKEN` built-in. Non introdurre nuovi secret senza segnalarlo esplicitamente. Mai token/credenziali nel README o nei workflow.

## Layout

| Path | Ruolo |
|------|-------|
| `README.md` | La pagina profilo. La maggior parte delle modifiche tocca solo questo. |
| `.github/workflows/` | Le automazioni che rigenerano parti del README. |
| `*.png`, `*.jpeg`, `*.jpg`, `_cover.PNG` | Icone social e banner referenziati dal README (serviti via `raw.githubusercontent.com`). |
| branch `output` / `assets-summary` / `assets-metrics` | Ex artefatti generati, **non più usati** dal 2026-10-13. |
| `.github/dependabot.yml` | **Unico** bot dipendenze (action raggruppate). `renovate.json` è disattivato (`"enabled": false`) per non avere PR duplicate. |

## Automazioni (non editare l'output generato a mano)

| Workflow | Scrive su | Trigger |
|----------|-----------|---------|
| `.github/workflows/update-readme.yml` | `README.md` (regione `BLOG-POST-LIST`, ultimi progetti) su master | ogni 6 h + push |
| `.github/workflows/stale.yml`, `issue-triage.yml` | issue e PR (label, stale/close) | pianificato / all'apertura |

## Trappole note / regole tecniche

- **Le Action che pushano richiedono** **Settings → Actions → General → Workflow permissions = "Read and write"**. Senza, falliscono in push.
- **Push umano che va stale**: committa su master solo `update-readme.yml` (ogni 6 h). Prima di pushare fare `git pull --rebase` (consigliato `git config pull.rebase true`).
- **Segreti**: nessun workflow ne usa più. `METRICS_TOKEN` e `TOKEN` (2020) restano nel repo inutilizzati, cancellabili da Settings → Secrets.
- **Immagini nel README**: repo images con URL raw completo (`https://raw.githubusercontent.com/Allan-Nava/Allan-Nava/master/<file>`) perché renderizzino sul profilo.
- **Badge**: usare `shields.io` stile `for-the-badge` per coerenza con la tech-stack row esistente.
- **Username per servizio**: `Allan-Nava` negli URL dei badge GitHub; `allannava` su dev.to; `allan__nava` su X/Twitter. Non confonderli.
- **Validazione**: nessun test. Prima di consegnare, preview del markdown; se tocchi un workflow valida lo YAML (`ruby -ryaml -e 'Dir[".github/workflows/*.yml"].each{|f| YAML.load_file(f)}'`).

## Puntatori

- Backlog operativo: `BACKLOG.md` · Milestone: `Profile v2` (#1) ex feature su secret, svuotata col profilo snello del 2026-10-13; `Profile v3` (#2) content & polish.
- Profilo: <https://github.com/Allan-Nava> · Sito: <https://allan-nava.github.io/> · dev.to: <https://dev.to/allannava>
- Servizi/servizi-Action usati: `skillicons.dev` (tech stack), `img.shields.io` (badge/stelle), `readme-typing-svg.demolab.com`, `gautamkrishnar/blog-post-workflow`. (Trophies e `github-readme-stats.vercel.app` rimossi: istanze pubbliche down — 402/503. Activity Graph, contatore visite, streak, metrics, snake, recent activity e WakaTime rimossi il 2026-10-13 per un profilo snello.)
- L'utente è **Allan Nava**, Software & DevOps Engineer @ HiWay Media (Milano). Repo di lavoro correlati: `devops_hiway`, `cnf-mng-hiway`.
