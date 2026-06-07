# Best practices memoire auto Claude Code

Reference exhaustive des regles que la skill applique. Issues du system prompt interne de Claude Code (section "auto memory").

## Les 4 types de memoire (strict)

| Type | Contenu | Quand sauvegarder |
|---|---|---|
| `user` | Role, preferences, expertise, contexte personnel de l'utilisateur | On apprend un detail sur l'utilisateur |
| `feedback` | Regle de comportement (correction OU validation) | L'utilisateur corrige ou valide une approche |
| `project` | Decision, deadline, motivation derriere un travail | On apprend un pourquoi non-trivial sur un projet |
| `reference` | Pointeur vers un systeme externe (Linear, Grafana, MCP path...) | On apprend ou trouver une info hors du repo |

Tout fichier qui ne rentre pas dans l'un de ces 4 types est suspect.

## Frontmatter obligatoire

```markdown
---
name: {{nom court}}
description: {{une ligne, specifique, sert a juger la pertinence future}}
type: {{user|feedback|project|reference}}
---
```

Champ `originSessionId` : tolere mais non standard, peut etre supprime.

## Body structure (feedback ET project uniquement)

Doit contenir :
- La regle ou le fait en premier
- Une ligne `**Why:**` (motivation, incident, contrainte)
- Une ligne `**How to apply:**` (quand/ou cette regle s'applique)

**Sans ces deux lignes, impossible de juger les cas limites plus tard.** C'est ce qui transforme une note en regle actionnable.

Pour les types `user` et `reference`, ces lignes ne sont pas obligatoires (l'info est descriptive, pas comportementale).

## MEMORY.md = pur index

Regles strictes :
- Pas de frontmatter
- Une ligne par memoire, format : `- [Title](file.md) — one-line hook`
- Chaque ligne < 150 caracteres
- Total < 200 lignes (au-dela = tronque par le harness)
- Aucun contenu de memoire ecrit directement (juste des pointeurs)
- Organisation semantique par sujet, pas chronologique

Sections autorisees : `## Theme` puis liste de pointeurs. Pas de sections multi-paragraphes.

## Ce qu'il NE FAUT PAS sauvegarder

| Categorie | Exemple |
|---|---|
| Patterns de code, architecture, chemins | "Le composant X est dans src/components/" |
| Historique git, qui-a-fait-quoi | "Refactor du 2026-04-14" sans pourquoi |
| Recettes de debug ou fixes | "Le bug X se fixe avec Y" |
| Doublons CLAUDE.md | Deja dans CLAUDE.md global ou local |
| Etat ephemere | Taches en cours, contexte de la conversation actuelle |
| Listes derivables d'un `ls` | "Skills existantes : a, b, c..." |
| Resumes du filesystem | Architecture d'une skill (les fichiers existent dans la skill) |

Ces exclusions s'appliquent **meme si l'utilisateur a explicitement demande la sauvegarde**. Dans ce cas, demander ce qui etait *surprenant* ou *non-obvious* — c'est ca qui merite memoire.

## Dates

Toujours absolues (`2026-04-28`), jamais relatives (`hier`, `la semaine derniere`, `recemment`).

Conversion automatique au moment de la sauvegarde initiale, jamais retroactivement (sinon perte d'info).

## Reference vs derivable

Test du "ou trouver l'info" :
- Info trouvable par `ls` ou lecture de fichier local → derivable, pas memoire
- Info trouvable par `git log` ou `git blame` → derivable, pas memoire
- Info trouvable dans CLAUDE.md → deja en contexte, pas memoire
- Info trouvable nulle part dans le repo → candidat memoire

Cas limite : info technique stockee sur un VPS/serveur distant → memoire legitime (l'utilisateur n'est pas toujours connecte au VPS).

## Cycle de vie

- **Creer** : seulement quand l'info est non-derivable, durable, actionnable
- **Mettre a jour** : verifier qu'aucune memoire existante couvre deja le sujet avant d'en creer une nouvelle
- **Supprimer** : quand l'info devient obsolete OU est devenue derivable (ex. ajoutee a CLAUDE.md)
- **Verifier** : avant de citer une memoire dans une recommandation, confirmer que les chemins/refs existent encore
