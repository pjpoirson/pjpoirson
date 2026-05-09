# Pipeline éditorial Corruption-Z — Mode d'emploi

Documentation des commandes du pipeline de bout en bout : du calendrier
éditorial à la publication automatique avec images.

---

## Vue d'ensemble

```
┌─────────────────────┐
│  1. Calendrier CSV  │  ← Tu prépares la liste des 120 articles
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  2. Génération MD   │  ← Claude Code génère les .md (texte + prompts MJ)
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  3. Images MJ       │  ← Tu produis les images dans Midjourney
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  4. Import WebP     │  ← Le script convertit et place les images
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  5. Build & Deploy  │  ← Astro build, push GitHub, Cloudflare déploie
└─────────────────────┘
```

---

## Pré-requis (à vérifier une fois)

Tu dois avoir installé sur ta machine :

- **Node.js** + **npm** (déjà OK puisque tu fais de l'Astro)
- **Claude Code** : `npm install -g @anthropic-ai/claude-code`
- **cwebp** pour la conversion d'images : `brew install webp`
- **Git** correctement configuré avec ton repo Cloudflare

Ton projet doit avoir :

- `lore/` à la racine avec tous tes fichiers de lore (manifest, bestiaire, cultes, etc.)
- `lore/midjourney-style.md` (style guide pour les prompts d'images)
- `.claude/commands/generate-seo-article.md` (la slash-command)
- `seo-pipeline/calendrier_editorial_corruption_z.csv` (le calendrier)
- `seo-pipeline/mj-workflow.sh`, `mj-prompts.sh`, `mj-import.sh` (les scripts d'images)
- `src/content.config.ts` patché avec le filtre `pubDate <= now` sur les listings publics
- Cloudflare Pages connecté avec un Deploy Hook + Worker cron actif

---

## ÉTAPE 1 — Préparer le calendrier éditorial

Le fichier `seo-pipeline/calendrier_editorial_corruption_z.csv` contient une
ligne par article à générer.

**Colonnes obligatoires (dans cet ordre) :**

1. Semaine de publication (1 à 120)
2. Titre
3. Mot-clé principal
4. Mots-clés secondaires (séparés par `;`)
5. Intention de recherche
6. Audience cible
7. Description rapide
8. **Slug** (le nom de fichier final, en kebab-case)
9. Image Heros conseillée (description)
10. Image 1 (description)
11. Image 2 (description)
12. **Pilier** (catégorisation : `I - Marée & Sariel`, `II - Cultes`, `III - Bestiaire`, etc.)
13. Fragment concerné (uniquement pour les articles narratifs)

**Règles importantes :**

- Le slug doit être **unique** dans le calendrier
- Les valeurs sont entourées de guillemets `"..."` pour gérer les virgules internes
- Le pilier détermine la `category` Astro et le **type narratif** (GUIDE / FRAGMENT / ESSAI)

**Vérification du calendrier :**

```bash
# Compter les articles
wc -l seo-pipeline/calendrier_editorial_corruption_z.csv

# Lister les piliers utilisés
python3 -c "
import csv
with open('seo-pipeline/calendrier_editorial_corruption_z.csv') as f:
    for row in csv.DictReader(f):
        print(row['Pilier'])
" | sort -u
```

---

## ÉTAPE 2 — Générer les articles avec Claude Code

### Avant de commencer

```bash
# Aller à la racine du projet
cd ~/projets/Corruption-Z-public-website

# Travailler sur une branche dédiée
git checkout -b auto/seo-batch-corruption-z
# (ou git checkout auto/seo-batch-corruption-z si la branche existe déjà)

# Empêcher le Mac de dormir pendant la génération
caffeinate -i &
```

### Lancer Claude Code en interactif

```bash
claude
```

### Générer par lots dans la session

Dans la session Claude Code, copie-colle ce prompt en adaptant la plage :

```
Génère les articles des semaines 5 à 15 du calendrier éditorial Corruption-Z, dans l'ordre.

Pour chaque semaine, applique scrupuleusement la slash-command /generate-seo-article.

Procédure pour chaque article :
1. Lis la ligne correspondante dans seo-pipeline/calendrier_editorial_corruption_z.csv
2. Vérifie que src/content/blog/<slug>.md n'existe pas déjà
3. Génère l'article complet selon les règles de la slash-command
4. Mets à jour seo-pipeline/tracker.csv

Continue même si un article échoue. À la fin, fais un récap : nombre d'articles générés, nombre d'échecs, liste des slugs créés.

Important : lis le contexte projet (lore + src/content.config.ts + articles existants) UNE SEULE FOIS au début, pas à chaque article.
```

### Lots recommandés

Fais des lots de 15 à 25 articles, avec une session Claude Code propre par lot
(ferme et rouvre `claude` entre chaque lot pour repartir d'un contexte vierge) :

| Lot   | Plage    | Articles | Durée approx. |
|-------|----------|----------|---------------|
| 1     | 1-15     | 15       | ~30 min       |
| 2     | 16-35    | 20       | ~30 min       |
| 3     | 36-55    | 20       | ~30 min       |
| 4     | 56-75    | 20       | ~30 min       |
| 5     | 76-95    | 20       | ~30 min       |
| 6     | 96-120   | 25       | ~40 min       |

### Vérifier après chaque lot

```bash
# Combien d'articles dans content/blog/
ls src/content/blog/ | wc -l

# Voir le tracker
cat seo-pipeline/tracker.csv

# Build Astro pour valider
npm run build
```

### Commit après chaque lot validé

```bash
git add -A
git commit -m "feat(seo): articles semaines X à Y"
git push origin auto/seo-batch-corruption-z
```

### Une fois les 120 articles générés

```bash
# Merge sur main
git checkout main
git merge auto/seo-batch-corruption-z
git push origin main

# Cloudflare déploie automatiquement
```

---

## ÉTAPE 3 — Produire les images avec Midjourney

Les articles sont en ligne, mais les images sont des placeholders cassés.
Tu vas les produire au fil de l'eau, **un article à la fois**.

### Le script principal

```bash
./seo-pipeline/mj-workflow.sh <slug>
```

**Exemple :**

```bash
./seo-pipeline/mj-workflow.sh marcheur-errante-damne-corruption-z
```

### Comment trouver le slug d'un article

Le slug est dans l'URL de l'article sur ton site :

```
https://corruption-z.com/marcheur-errante-damne-corruption-z/
                          ↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑
                                  copie ça
```

Tu peux aussi le voir dans la page `/lore/` sur ton site, ou dans le frontmatter
`slug:` de chaque fichier `.md`.

### Ce que le script fait

1. **Vérifie que l'article existe** (sinon te suggère des slugs proches)
2. **Alerte si des images existent déjà** et te demande si tu veux écraser
3. **Affiche les prompts Midjourney** à copier-coller dans Midjourney
4. **Attend que tu télécharges tes images** dans `~/Downloads/`
5. **Convertit les images en WebP** et les place dans `public/images/blog/`
6. **Lance le build** et te donne l'URL pour vérifier

### Convention de nommage des images dans ~/Downloads/

Quand tu télécharges depuis Midjourney, renomme tes fichiers ainsi :

| Image                       | Nom du fichier                    |
|-----------------------------|-----------------------------------|
| Hero (en haut de l'article) | `<slug>-hero.png`                 |
| 1ère image du corps         | `<slug>-1.png`                    |
| 2ème image du corps         | `<slug>-2.png`                    |
| 3ème image (si présente)    | `<slug>-3.png`                    |

Le script accepte aussi `.jpg`, `.jpeg`, `.webp` en entrée.

### Workflow complet pour un article

```bash
# 1. Lance le script avec le slug
./seo-pipeline/mj-workflow.sh marcheur-errante-damne-corruption-z

# 2. Le script affiche les prompts MJ. Copie-les dans Midjourney avec ta ref.

# 3. Génère, valide visuellement, télécharge dans ~/Downloads/

# 4. Renomme les fichiers selon la convention ci-dessus

# 5. Reviens dans le terminal et appuie sur Entrée

# 6. Le script convertit, importe, et lance npm run build

# 7. Va sur http://localhost:4321/<slug>/ pour vérifier
```

### Commit des images

Une fois l'article validé visuellement :

```bash
git add public/images/blog/<slug>-*.webp
git commit -m "feat(images): images pour <slug>"
git push
```

Cloudflare déploie automatiquement.

---

## ÉTAPE 4 — Cycle de publication automatique

À partir du **11 mai 2026** (date de la première pubDate "future" : article semaine 16),
les articles se publient tout seuls grâce à :

- Le filtre `pubDate <= now` dans les pages de listing
- Le cron Cloudflare qui rebuild le site tous les **lundis et jeudis à 8h UTC**

**Tu n'as rien à faire** côté publication. Tu produis juste les images au fil
de l'eau pour que les articles soient illustrés au moment de leur sortie.

---

## Annexes

### Vérifier l'état d'avancement

```bash
# Articles totaux dans content/blog/
ls src/content/blog/*.md | wc -l

# Articles avec image hero (en .webp)
ls public/images/blog/*-hero.webp 2>/dev/null | wc -l

# Articles dans le tracker
wc -l seo-pipeline/tracker.csv
```

### Lister les articles sans image hero

```bash
for f in src/content/blog/*.md; do
  slug=$(basename "$f" .md)
  if [[ ! -f "public/images/blog/${slug}-hero.webp" ]]; then
    echo "$slug"
  fi
done
```

### Forcer un rebuild Cloudflare manuellement

```bash
curl -X POST "TON-URL-DEPLOY-HOOK"
```

(L'URL est dans Cloudflare → Pages → ton projet → Settings → Builds & deployments → Deploy hooks)

### Régénérer un article spécifique

Si tu veux refaire un article du calendrier :

```bash
# Supprimer l'ancien
rm src/content/blog/<slug>.md

# Modifier seo-pipeline/tracker.csv : passer la ligne à "todo" ou la supprimer

# Lancer Claude Code en interactif et taper :
/generate-seo-article <numéro-de-semaine>
```

### Si quelque chose plante

- **Build Astro KO** : `npm run build 2>&1 | grep -A 3 "error"` pour identifier l'article fautif
- **Image non trouvée** : vérifie le nommage exact dans `~/Downloads/`
- **cwebp introuvable** : `brew install webp`
- **Permission denied sur ~/Downloads/** : Réglages Système → Confidentialité → Accès complet au disque → ajouter Terminal/cmux/Warp

---

## Récap du flow type sur une semaine

**Lundi soir** :
1. Tu choisis 3-5 articles à illustrer
2. Pour chacun : `./seo-pipeline/mj-workflow.sh <slug>`
3. Tu copies les prompts dans Midjourney, tu valides, tu télécharges
4. Le script importe et build automatiquement
5. `git add . && git commit -m "feat(images): X articles" && git push`

**Cloudflare** déploie en 2-3 minutes. Le cron du mardi/jeudi prendra le relais
pour publier les articles dont la pubDate arrive à échéance.

C'est tout. Le pipeline tourne tout seul à partir de là.

---

*Dernière mise à jour : 8 mai 2026*