---
name: memory-reorganize
description: "Audite et reorganise la memoire auto de Claude Code (~/.claude/projects/<workspace>/memory/) selon les best practices du systeme : 4 types stricts (user/feedback/project/reference), format Why/How to apply, MEMORY.md comme pur index, dates absolues, pas de duplicats, pas de contenu derivable du filesystem ou de git. Detecte les violations, propose un plan de reorganisation, applique apres validation. Utiliser cette skill des que l'utilisateur veut nettoyer, reorganiser, auditer ou refactorer sa memoire, ou quand il dit que sa memoire est sale ou desordonnee."
user-invocable: true
argument-hint: "[optionnel : focus sur un aspect specifique ex. 'duplicats', 'frontmatter', 'index']"
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep
  - AskUserQuestion
---

# Memory Reorganize

Audite la memoire auto de Claude Code et la reorganise selon les best practices definies dans le system prompt. Detecte les violations, propose un plan, applique apres validation utilisateur.

**Repertoire cible** : `~/.claude/projects/<workspace>/memory/`

Le `<workspace>` est l'identifiant encode du dossier de travail (ex. pour `~/.claude/skills`, ce sera `-home-user--claude-skills`). Lister `~/.claude/projects/` pour identifier le bon dossier au moment de l'invocation.

## Fichiers de reference (charger selon besoin)

- **[BEST_PRACTICES.md](BEST_PRACTICES.md)** — regles completes : 4 types, frontmatter, body Why/How, MEMORY.md = index, exclusions
- **[EXAMPLES.md](EXAMPLES.md)** — refactos avant/apres concrets et table des anti-patterns

Lire `BEST_PRACTICES.md` AVANT la phase 2 (audit). Lire `EXAMPLES.md` quand on rentre en phase 8 (plan de refacto) pour calibrer le style.

## Progression

Copier et suivre cette checklist :

```
- [ ] Phase 1 : Inventaire (lister fichiers, lire MEMORY.md)
- [ ] Phase 2 : Audit individuel de chaque fichier memoire (vs BEST_PRACTICES.md)
- [ ] Phase 3 : Audit de MEMORY.md (format index)
- [ ] Phase 4 : Detection de duplicats et chevauchements
- [ ] Phase 5 : Detection de contenu derivable (filesystem, git, CLAUDE.md)
- [ ] Phase 6 : Verification de fraicheur (refs cassees, dates relatives)
- [ ] Phase 7 : Rapport des violations a l'utilisateur
- [ ] Phase 8 : Plan de reorganisation (consulter EXAMPLES.md)
- [ ] Phase 9 : Validation utilisateur (AskUserQuestion)
- [ ] Phase 10 : Backup + application + verification finale
```

Si `$ARGUMENTS` contient un focus (ex. "duplicats", "frontmatter"), prioriser les phases concernees mais executer quand meme l'inventaire (phase 1-2).

## Phase 1 : Inventaire

```bash
# Identifier le workspace de memoire actif
ls ~/.claude/projects/

# Puis sur le bon dossier :
ls -la "~/.claude/projects/<workspace>/memory/"
wc -l "~/.claude/projects/<workspace>/memory/"*.md
```

Noter :
- Nombre de fichiers memoire
- Taille de MEMORY.md (lignes)
- Orphelins (fichier present mais absent de MEMORY.md)
- Refs cassees (lien dans MEMORY.md vers fichier absent)

## Phase 2 : Audit individuel

Pour chaque fichier (autre que MEMORY.md), verifier contre `BEST_PRACTICES.md` :

1. Frontmatter present et complet (`name`, `description`, `type`)
2. `type` dans {user, feedback, project, reference}
3. Si type=feedback ou type=project : lignes `**Why:**` et `**How to apply:**` presentes
4. Description specifique, pas generique
5. Dates absolues (pas "recemment", "la semaine derniere")
6. Pas de contenu interdit (code/architecture/git derivable)
7. Coherence type ↔ contenu

Tenir un tableau des violations par fichier.

## Phase 3 : Audit de MEMORY.md

1. Pas de frontmatter
2. Chaque ligne d'entree < 150 caracteres
3. Format `- [Title](file.md) — hook` respecte
4. Pas de contenu de memoire ecrit directement (juste des liens)
5. Total < 200 lignes
6. Sections semantiques par sujet, pas chronologiques
7. Chaque entree pointe vers un fichier qui existe

## Phase 4 : Duplicats et chevauchements

Detecter :
- Deux fichiers couvrant le meme sujet
- Info qui apparait dans plusieurs fichiers
- Memoire qui devrait etre fusionnee avec une autre

Utiliser Grep pour trouver des mots-cles communs entre fichiers.

## Phase 5 : Contenu derivable

Pour chaque memoire, appliquer le test du "ou trouver l'info" (cf. BEST_PRACTICES.md > Reference vs derivable). Si derivable du filesystem / git / CLAUDE.md → candidat suppression ou reduction.

## Phase 6 : Fraicheur

- Memoires referencant des chemins/fichiers : verifier qu'ils existent encore (Bash `ls`)
- Memoires referencant MCP/env vars/services : verifier configuration actuelle
- Memoires datees > 6 mois : flagger pour revue manuelle

## Phase 7 : Rapport des violations

Format :

```markdown
## Audit de la memoire — [DATE]
**Etat actuel** : N fichiers, MEMORY.md = X lignes

### Violations critiques
- [Fichier] — Frontmatter manquant / type invalide / ligne >150 chars

### Violations moyennes
- [Fichier] — type=feedback sans Why/How / date relative

### Contenu derivable a supprimer
- MEMORY.md L8-19 — "Existing skills" derivable de `ls`

### Duplicats
- fileA.md et fileB.md couvrent le sujet X

### Refs cassees / Orphelins
- MEMORY.md L42 → fichier_inexistant.md
- foo.md present mais absent de MEMORY.md
```

## Phase 8 : Plan de reorganisation

Avant de proposer, consulter `EXAMPLES.md` pour calibrer le style des refactos.

Plan structure :
1. **Fichiers a supprimer** : liste + raison
2. **Fichiers a fusionner** : A+B → C avec justification
3. **Fichiers a refactorer** : ajout frontmatter / Why / How / conversion dates
4. **MEMORY.md** : version reecrite en sections semantiques

Montrer un diff conceptuel avant/apres pour MEMORY.md.

## Phase 9 : Validation utilisateur

Utiliser `AskUserQuestion` avec options :
- "Appliquer tout le plan"
- "Appliquer seulement les violations critiques"
- "Voir le detail d'un changement avant de decider"
- "Annuler"

Ne JAMAIS appliquer sans validation explicite.

## Phase 10 : Application

1. **Backup** : copier chaque fichier modifie en `*.backup-YYYY-MM-DD` AVANT modification
2. Supprimer les fichiers identifies (`rm`)
3. Fusionner (lire sources, ecrire cible, supprimer sources)
4. Refactorer (Edit pour frontmatter, Why/How, dates absolues)
5. Reecrire MEMORY.md en sections semantiques + entrees < 150 chars
6. **Verification finale** : relire MEMORY.md et chaque fichier modifie

Reporter en fin :
- N fichiers supprimes / fusionnes / refactores
- Nouvelle taille de MEMORY.md
- Chemin des backups

## Regles d'or

- **Ne jamais inventer** une memoire qui n'existe pas. Reorganiser, pas creer ex nihilo.
- **Preserver le contenu de valeur** : si une memoire viole les regles mais contient de l'info utile, refactorer plutot que supprimer.
- **Backup avant tout** : `*.backup-YYYY-MM-DD` avant la moindre modification.
- **Demander en cas de doute** : memoire ambigue → AskUserQuestion plutot que decider seul.
- **Ne pas toucher aux fichiers hors du repertoire memoire** : pas de modification de CLAUDE.md, settings.json, skills.
