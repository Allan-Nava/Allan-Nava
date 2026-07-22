# AGENTS.md — Allan-Nava

Repo **profilo GitHub** (`github.com/Allan-Nava/Allan-Nava`): il `README.md` viene renderizzato come landing page del profilo su <https://github.com/Allan-Nava>. **Nessun codice applicativo, build o test**: il "prodotto" è il `README.md` renderizzato + le GitHub Actions che lo tengono aggiornato in automatico.

Questo file definisce le regole operative per gli agent (Copilot, Claude, altri tool AI) quando lavorano in questo repository.

## Regole di lavoro (SEMPRE)

- **MAI `git push`**: lo fa sempre l'utente. **MAI committare senza che l'utente lo chieda esplicitamente.** MAI `Co-Authored-By` nei commit.
- **Commit gitmoji**: la history usa prefissi gitmoji-style (`:zap:`, `:robot:`, `:sparkles:`). Mantenere lo stile. I commit automatici delle Action usano `:robot:`.
- **Zone gestite dalle Action = intoccabili a mano**: il contenuto tra i marker HTML lo riscrive il workflow `update-readme.yml`. Mantenere i marker intatti, mai svuotarli/editarli a mano:
  - `<!--START_SECTION:activity-->` … `<!--END_SECTION:activity-->` (recent activity)
  - `<!-- BLOG-POST-LIST:START -->` … `<!-- BLOG-POST-LIST:END -->` (post dev.to)
  - `<!-- YOUTUBE-VIDEOS:START -->` … `<!-- YOUTUBE-VIDEOS:END -->` (video YouTube)
- **Artefatti generati stanno su branch dedicati, MAI su master** (così il push umano non va stale): snake → `output`, calendario 3D → `assets`, summary cards → `assets-summary`. Il README li referenzia via URL raw da quei branch. Non committare file generati su master.
- **Tema coerente SEMPRE**: accento verde `#10cf53` su sfondo nero `#050505`, testo bianco `#ffffff`. Ogni nuova stat card / badge / servizio va allineato a questa palette.
- **Allineare tutto**: ogni modifica fattuale (nuovo workflow, nuova sezione, nuovo servizio) va propagata a `README.md`, ai marker, a questo file e a `CLAUDE.md`.
- **Segreti**: tutti i workflow usano solo il `GITHUB_TOKEN` built-in. Non introdurre nuovi secret senza segnalarlo esplicitamente. Mai token/credenziali nel README o nei workflow.

## Layout

| Path | Ruolo |
|------|-------|
| `README.md` | La pagina profilo. La maggior parte delle modifiche tocca solo questo. |
| `.github/workflows/` | Le automazioni che rigenerano parti del README. |
| `*.png`, `*.jpeg`, `*.jpg`, `_cover.PNG` | Icone social e banner referenziati dal README (serviti via `raw.githubusercontent.com`). |
| branch `output` / `assets` / `assets-summary` | Artefatti **generati** (snake / 3D / summary), non su master. |
| `renovate.json`, `.github/dependabot.yml` | Config automazione dipendenze. |

## Automazioni (non editare l'output generato a mano)

| Workflow | Scrive su | Trigger |
|----------|-----------|---------|
| `.github/workflows/update-readme.yml` | `README.md` (regioni `activity`, `BLOG-POST-LIST`, `YOUTUBE-VIDEOS`) — **unico** che committa su master | ogni 6 h + push |
| `.github/workflows/snake.yml` | branch `output` (SVG/GIF) | ogni 12 h + push |
| `.github/workflows/profile-3d.yml` | branch `assets` (SVG 3D) | giornaliero |
| `.github/workflows/profile-summary.yml` | branch `assets-summary` (SVG) — **serve PAT** `GH_TOKEN_SUMMARY` | giornaliero |

## Trappole note / regole tecniche

- **Le Action che pushano richiedono** **Settings → Actions → General → Workflow permissions = "Read and write"**. Senza, falliscono in push.
- **Push umano che va stale**: solo `update-readme.yml` committa su master (in un unico burst ogni 6 h). Prima di pushare fare `git pull --rebase` (consigliato `git config pull.rebase true`). Snake/3D/summary stanno su branch dedicati e NON toccano master.
- **Le immagini snake/3D compaiono solo dopo il primo run** del workflow (branch `output`/`assets` inizialmente assenti). Lanciabili da *Actions → Run workflow*.
- **Immagini nel README**: repo images con URL raw completo (`https://raw.githubusercontent.com/Allan-Nava/Allan-Nava/master/<file>`) perché renderizzino sul profilo; file **generati** via URL raw dal loro branch (`.../Allan-Nava/output/<file>` per lo snake, `.../Allan-Nava/assets/<file>` per il 3D).
- **Badge**: usare `shields.io` stile `for-the-badge` per coerenza con la tech-stack row esistente.
- **Username per servizio**: `Allan-Nava` negli URL delle stat card; `allannava` su dev.to; `allan__nava` su X/Twitter. Non confonderli.
- **Validazione**: nessun test. Prima di consegnare, preview del markdown; se tocchi un workflow valida lo YAML (`ruby -ryaml -e 'Dir[".github/workflows/*.yml"].each{|f| YAML.load_file(f)}'`).

## Puntatori

- Profilo: <https://github.com/Allan-Nava> · Sito: <https://allan-nava.github.io/> · dev.to: <https://dev.to/allannava>
- Servizi stat usati: `github-readme-stats.vercel.app`, `github-readme-streak-stats.herokuapp.com`, `github-readme-activity-graph.vercel.app`, `readme-typing-svg.demolab.com`, `Platane/snk`, `yoshi389111/github-profile-3d-contrib`, `gautamkrishnar/blog-post-workflow`. (Trophies rimosse: il servizio hosted `github-profile-trophy.vercel.app` è down con 402/DEPLOYMENT_DISABLED.)
- L'utente è **Allan Nava**, DevOps Engineer @ HiWay Media (Milano). Repo di lavoro correlati: `devops_hiway`, `cnf-mng-hiway`.
