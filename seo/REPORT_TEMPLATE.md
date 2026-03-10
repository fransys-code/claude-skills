# Modele de Rapport d'Audit SEO

Adapter selon le mode (quick/full/eeat).

---

## Mode rapide (Quick Audit)

```markdown
# Verification SEO Rapide

Date : [DATE]
Cible : [CHEMIN-OU-REPERTOIRE]

---

## Resume executif

**Etat general** : [Excellent / Bon / Satisfaisant / A ameliorer / Urgent]

**Fichiers verifies** : X fichiers (Y excellents, Z a ameliorer, W critiques)

**Probleme principal** : [Decrire le probleme le plus impactant]

---

## Elements conformes

- [Liste des pages/elements sans probleme]

---

## Points a ameliorer

### 1. [Element] - Priorite HAUTE
**Impact** : [Decrire l'impact]
**Correction** : [Instruction concrete]
**Exemple** :
<!-- Avant -->
[Code existant]
<!-- Apres -->
[Code corrige]

### 2. [Element] - Priorite MOYENNE

### 3. [Element] - Priorite BASSE

---

## Recommandations immediates

1. **Action 1** : [Description] — Effort : [Bas/Moyen/Eleve] — Impact : [Bas/Moyen/Eleve]
2. **Action 2** : [Description]
3. **Action 3** : [Description]

---

Pour un audit plus approfondi :
- `/seo . --mode full` pour analyse complete avec scoring
- `/seo --mode eeat` pour audit E-E-A-T detaille
- `/seo-geo` pour optimisation moteurs de recherche IA
```

---

## Mode complet (Full Audit)

```markdown
# Audit SEO Complet

**Score global : [XX]/100**

**Evaluation** : [Excellent / Bon / Satisfaisant / A ameliorer / Urgent]

Date : [DATE]
Projet : [CHEMIN-RACINE]
Pages analysees : [X pages, Y composants]
Langue detectee : [LANGUE]
Technologie : [Next.js App Router / Pages Router / Autre]

---

## Scores detailles par dimension

### 1. Completude des metadonnees : [XX]/18
- Title : [X/6]
- Meta Description : [X/7]
- Open Graph : [X/3]
- Social Cards : [X/2]

### 2. Donnees structurees : [XX]/13
- JSON-LD : [X/6]
- Champs obligatoires : [X/4]
- Validation : [X/3]

### 3. Qualite du contenu : [XX]/22
- Structure des titres : [X/5]
- Longueur du contenu : [X/5] — Moyenne : [XXX] mots
- Information Gain : [X/4]
- Qualite des liens : [X/3]
- Multimedia : [X/3]
- Extractibilite IA : [X/2]

### 4. Autorite E-E-A-T : [XX]/18
- Experience : [X/6]
- Expertise : [X/5]
- Autorite : [X/4]
- Fiabilite : [X/3]

### 5. Strategie de contenu : [XX]/10
- Structure en clusters : [X/4]
- Calendrier editorial : [X/3]
- Cannibalisation : [X/3]

### 6. SEO technique : [XX]/10
- robots.txt & Sitemap : [X/2]
- Optimisation images : [X/2]
- Structure URLs : [X/2]
- Mobile & Core Web Vitals : [X/3]
- HTTPS : [X/1]

### 7. Visibilite IA : [XX]/9
- Crawlers IA : [X/3]
- llms.txt : [X/2]
- Contenu extractible : [X/2]
- Entity linking : [X/2]

---

## Problemes critiques

### P1 - [Probleme 1]
**Description** : [Explication]
**Fichiers affectes** : [Exemples]
**Correction** : [Instructions]
**Impact** : [+X points au score]

---

## Suggestions par priorite

### Priorite HAUTE (+3 points ou plus)
1. [Probleme] — Correction : [Instruction] — Effort : [Bas/Moyen/Eleve]

### Priorite MOYENNE (+1-2 points)
1. [Probleme] — Correction : [Instruction]

### Priorite BASSE (+1 point)
1. [Probleme] — Correction : [Instruction]

---

## Feuille de route

### Semaine 1-2 : Foundation (Gain : +[X] points)
- [ ] [Actions critiques]

### Semaine 3-4 : Optimisation (Gain : +[Y] points)
- [ ] [Actions]

### Semaine 5+ : Amelioration continue (Gain : +[Z] points)
- [ ] [Actions long terme]

**Projection** : Score actuel [XX]/100 > Score cible [YY]/100
```

---

## Mode E-E-A-T

```markdown
# Audit E-E-A-T

**Score E-E-A-T : [XX]/18**

**Secteur** : [Secteur d'activite]
**Sensibilite** : [General / YMYL (sante, finance, actualites)]

Date : [DATE]
Pages analysees : [X pages]

---

## 1. Experience : [XX]/6
- Etudes de cas : [X presentes]
- Partage d'experience : [Evaluation]
- Profondeur technique : [Evaluation]

## 2. Expertise : [XX]/5
- Biographie d'auteur : [Presente/Absente]
- Accreditations : [Details]
- Langage professionnel : [Evaluation]

## 3. Autorite : [XX]/4
- Contenu unique : [Evaluation]
- Citations externes : [Details]

## 4. Fiabilite : [XX]/3
- Transparence : [Page A propos, mentions legales, contact]
- Fraicheur : [Frequence de mise a jour]
- Sources : [Nombre et qualite]

---

## Plan d'action

### Immediat (Semaine 1-2)
1. [ ] [Action a fort impact]

### Court terme (Mois 1)
1. [ ] [Action]

### Long terme (Mois 2-3)
1. [ ] [Action continue]

**Projection** : Score actuel [XX]/18 > Score cible [YY]/18
```

---

## Notes de remplissage

- `[XX]` : Remplacer par score numerique
- `[TEXTE]` : Remplacer par contenu pertinent
- Dates en format : DD/MM/YYYY
