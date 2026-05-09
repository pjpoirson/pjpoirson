# ❡ Corruption-Z

**Un jeu vidéo et un univers fictionnel développé avec l'IA, sous direction humaine stricte.**

Survival-horror gothique-cosmique. Dans un monde où la chute d'un ange a fissuré la trame du réel il y a des millénaires, l'humanité de 2050 survit dans les ruines d'une apocalypse spirituelle dont la marée recouvre lentement, inexorablement, ce qui restait debout. Pas de monstres conçus, pas de virus, pas de cyberpunk : seulement la pierre humide, l'or terni, et les chœurs liturgiques qui montent de sous les églises.

→ **[corruption-z.com ↗](https://corruption-z.com)**

---

## Méthode

Projet personnel mené en solo avec l'IA comme partenaire de production sur l'ensemble de la chaîne créative. La méthode tient en une phrase : **un manifeste figé en amont, un lore qui en découle, et tous les volets en aval (visuel, design, musique, pipeline éditorial) qui s'y alignent sans dériver.** La cohérence tonale est obtenue par garde-fous explicites intégrés dans chaque prompt et chaque slash-command — jamais laissée à l'improvisation du modèle.

## Chiffres-clés

- **5 documents de lore central** + manifeste de vision
- **120 fragments narratifs** (100 fragments classiques + 20 Voix de la Marée)
- **22 cartes de tarot** in-game documentées avec prompts visuels
- **38 pictogrammes** custom + 6 familles typographiques dans le design system
- **120 articles SEO** générés et planifiés sur 2 ans, publiés automatiquement
- **2 builds par semaine** déclenchés sans intervention manuelle

---

## Définition de la vision et cascade de production

```
        ┌──────────────────────────────────┐
        │  PROJET INITIAL INSATISFAISANT   │
        │   (identité diluée, hésitante)   │
        └─────────────────┬────────────────┘
                          │
                          ▼
                ┌───────────────────┐
                │     MANIFESTE     │
                │   (refondation)   │
                └─────────┬─────────┘
                          │
                          ▼
                ┌───────────────────┐
                │       LORE        │
                │    (la source)    │
                └─────────┬─────────┘
                          │
            ┌─────────────┼─────────────┐
            ▼             ▼             ▼
       ┌─────────┐   ┌─────────┐   ┌─────────┐
       │ VISUEL  │   │ DESIGN  │   │MUSIQUE  │
       │         │   │ SYSTEM  │   │         │
       └────┬────┘   └────┬────┘   └────┬────┘
            │             │             │
            └─────────────┴─────────────┘
                          │
                  alimentent les deux
                          │
            ┌─────────────┴─────────────┐
            ▼                           ▼
      ┌──────────┐              ┌──────────────┐
      │  LE JEU  │              │ SITE PUBLIC  │
      └──────────┘              └──────▲───────┘
                                       │
                                ┌──────┴───────┐
                                │   PIPELINE   │
                                │  ÉDITORIAL   │
                                └──────────────┘
```

*Le manifeste a été écrit en réaction à un projet initial dont l'identité ne convergeait pas. Il pose les règles non-négociables qui filtrent toute production en aval. Le lore en découle. Trois directions parallèles (visuel, design system, musique) en sortent et alimentent **deux destinations** : le jeu et le site public. Le pipeline éditorial est le mécanisme qui produit les 120 articles du site, sur la base du lore, du visuel et du design system.*

---

## Les six volets

### ❡ Création du manifeste

→ **[01-manifeste.md](./01-manifeste.md)** · *publié intégralement*

Le document de cadrage absolu, écrit en réaction à un projet initial à l'identité diluée. Cinq piliers tonals non-négociables (Sacré Profané, Pierre vs Acier, Oppression Sonore, Réalité Qui Se Déchire, Fureur ET Mélancolie), tests de validité d'une idée, et le garde-fou anti-cyberpunk qui se colle à chaque prompt IA pour empêcher toute dérive techno-futuriste. C'est le filtre qui rend possible tout le reste.

### ❡ Création du lore

→ **[02-lore.md](./02-lore.md)** · *condensé public + fragments teasers*

Le lore complet contient des révélations cachées aux joueurs et n'est pas publié. Cette page propose un **condensé narratif accessible** de l'univers (Sariel, la chute, les cultes, le monde de 2050) plus une sélection de **5 à 10 fragments narratifs** choisis comme teasers, dont l'histoire complète se découvre dans le jeu et sur [corruption-z.com](https://corruption-z.com).

### ❡ Direction visuelle

→ **[03-direction-visuelle.md](./03-direction-visuelle.md)** · *méthode + prompts*

Pierre humide, or terni, rouge sang séché, noir profond. Syncrétisme orthodoxe + apocryphes + gnosticisme + paganisme slave + énochien. Inspirations : Bloodborne, Berserk, Hellblade, Silent Hill 2, The Witch, Control. Cette page documente la **méthode de production avec Midjourney** — palette, références (Caravaggio, Beksinski, Wyeth, Friedrich), usage de `--sref` pour la cohérence du tarot, structure de prompts type — avec **exemples concrets** d'illustrations produites pour le jeu et pour le site éditorial.

### ❡ Direction musicale

→ **[04-direction-musicale.md](./04-direction-musicale.md)** · *méthode + extraits*

Bibliothèque musicale structurée par catégories (PREGAME, EXPLORATION, INDOOR, DANGER, COMBAT, BOSS, DEATH) avec règles de mixage et garde-fous explicites contre Hans Zimmer, l'héroïsme triomphant, et la techno-distorsion. Cette page documente la **méthode Suno** : prompts type par catégorie, structure des chœurs orthodoxes, latin déformé, cloches, nappes ambient, et **extraits audio** des morceaux validés.

### ❡ Création du design system et intégration

→ **[05-design-system.md](./05-design-system.md)** · *méthode + tokens*

Design System v3.7 « Marée & Spectre ». Tokens chromatiques (7 familles), typographiques (6 familles avec rôles uniques), 38 pictogrammes custom, lettrines, composants HUD complets du gameplay, et 6 fenêtres modales spécifiées. Cette page documente la **méthode de conception itérative avec Claude** (versionnage v1 → v3.7, gouvernance, anti-patterns), un **extrait des tokens**, et l'intégration au site public Astro et au HUD du jeu.

### ❡ Création d'une pipeline éditoriale automatisée

→ **[06-pipeline-editorial.md](./06-pipeline-editorial.md)** · *architecture complète*

**120 articles SEO** générés par Claude Code en mode batch via slash-command custom, sur la base d'un calendrier éditorial CSV et d'un dossier `lore/` lu en imprégnation systématique. Stockés en Astro Content Collections avec validation Zod, publiés automatiquement par filtre `pubDate <= now()`, déployés par Cloudflare Pages, build déclenché 2 fois par semaine via Worker + Cron Trigger. **Le volet techniquement le plus dense du projet** — architecture complète documentée avec schéma, code et chiffres réels.

---

*Documentation en cours de complétion. Chaque page est rédigée au fil de l'eau et reflète l'état réel de la production à sa date de mise à jour.*

---

*Dernière mise à jour : 8 mai 2026*