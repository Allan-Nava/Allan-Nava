# CLAUDE.md — Allan-Nava

Repo **profilo GitHub** (`github.com/Allan-Nava/Allan-Nava`): il `README.md` è renderizzato come landing page del profilo su <https://github.com/Allan-Nava>. **Nessun codice applicativo, build o test**: il "prodotto" è il `README.md` renderizzato + le GitHub Actions che lo aggiornano in automatico.

## Regole di lavoro (SEMPRE)

- **MAI `git push`** — lo fa sempre l'utente. **MAI committare senza richiesta esplicita.** MAI `Co-Authored-By` nei commit.
- **Commit gitmoji** — la history usa prefissi gitmoji-style (`:zap:`, `:robot:`, `:sparkles:`). Mantenere lo stile; i commit automatici delle Action usano `:robot:`.
- **Zone gestite dalle Action = intoccabili a mano** — il contenuto tra i marker HTML lo riscrivono i workflow. Mantenere i marker intatti, mai svuotarli/editarli a mano:
  - `<!--START_SECTION:activity-->` … `<!--END_SECTION:activity-->` (recent activity)
  - `<!-- BLOG-POST-LIST:START -->` … `<!-- BLOG-POST-LIST:END -->` (post dev.to)
- **File generati = intoccabili a mano** — `profile-3d-contrib/` (calendario 3D) e il branch `output` (snake) sono rigenerati dalle Action. Non editarli né committarli a mano.
- **Tema coerente SEMPRE** — accento verde `#10cf53` su sfondo nero `#050505`, testo bianco `#ffffff`. Ogni nuova stat card / badge / servizio va allineato alla palette.
- **Allineare tutto** — ogni modifica fattuale (nuovo workflow, nuova sezione, nuovo servizio) va propagata a `README.md`, ai marker, a `AGENTS.md` e a questo file.
- **Segreti** — tutti i workflow usano solo il `GITHUB_TOKEN` built-in. Non introdurre nuovi secret senza segnalarlo. Mai token/credenziali in README o workflow.

## Layout

| Path | Ruolo |
|------|-------|
| `README.md` | La pagina profilo. La maggior parte delle modifiche tocca solo questo. |
| `.github/workflows/` | Le automazioni che rigenerano parti del README. |
| `*.png`, `*.jpeg`, `*.jpg`, `_cover.PNG` | Icone social e banner referenziati dal README (via `raw.githubusercontent.com`). |
| `profile-3d-contrib/` | SVG del calendario 3D — **generati**, non editare a mano. |
| `renovate.json`, `.github/dependabot.yml` | Config automazione dipendenze. |

## Automazioni (non editare l'output generato a mano)

| Workflow | Scrive su | Trigger |
|----------|-----------|---------|
| `.github/workflows/UPDATE-READMEV2.yml` | `README.md`, regione `activity` | ogni 30 min + push |
| `.github/workflows/snake.yml` | branch `output` (SVG/GIF) | ogni 12 h + push |
| `.github/workflows/profile-3d.yml` | cartella `profile-3d-contrib/` | giornaliero |
| `.github/workflows/blog-posts.yml` | `README.md`, regione `BLOG-POST-LIST` | orario |

## Trappole note / regole tecniche

- **Snake e 3D scrivono sul repo** → richiedono **Settings → Actions → General → Workflow permissions = "Read and write"**. Senza, le Action falliscono in push.
- **Le immagini snake/3D compaiono solo dopo il primo run** del workflow (branch `output` e cartella `profile-3d-contrib/` inizialmente assenti). Lanciabili da *Actions → Run workflow*.
- **Immagini nel README** — repo images con URL raw completo (`https://raw.githubusercontent.com/Allan-Nava/Allan-Nava/master/<file>`) perché renderizzino sul profilo; file generati con path repo-relativo (`./profile-3d-contrib/...`).
- **Badge** — usare `shields.io` stile `for-the-badge` per coerenza con la tech-stack row esistente.
- **Username per servizio** — `Allan-Nava` negli URL delle stat card; `allannava` su dev.to; `allan__nava` su X/Twitter. Non confonderli.
- **Validazione** — nessun test. Preview del markdown prima di consegnare; se tocchi un workflow valida lo YAML (`ruby -ryaml -e 'Dir[".github/workflows/*.yml"].each{|f| YAML.load_file(f)}'`).

## Puntatori

- Profilo: <https://github.com/Allan-Nava> · Sito: <https://allan-nava.github.io/> · dev.to: <https://dev.to/allannava>
- Servizi stat usati: `github-readme-stats.vercel.app`, `github-readme-streak-stats.herokuapp.com`, `github-profile-trophy.vercel.app`, `github-readme-activity-graph.vercel.app`, `Platane/snk`, `yoshi389111/github-profile-3d-contrib`, `gautamkrishnar/blog-post-workflow`.
- L'utente è **Allan Nava**, DevOps Engineer @ HiWay Media (Milano). Repo di lavoro correlati: `devops_hiway`, `cnf-mng-hiway`.
