# 06 — Création d'une pipeline éditoriale automatisée

← [Retour à l'index](./README.md)

---

## En une phrase

**120 articles SEO générés par Claude Code en mode batch, publiés automatiquement sur Astro + Cloudflare Pages, avec un rebuild hebdomadaire piloté par cron.**

Le volet techniquement le plus dense du projet, et celui qui montre le mieux la méthode : **un humain qui pilote l'IA**, pas l'inverse.

---

## I. Le récit de conception

### Le problème de départ

Le calendrier éditorial v2 contenait **120 articles** planifiés sur 2 ans. Une rédaction manuelle au rythme de 2 articles par semaine aurait demandé un an de travail soutenu, sans compter la production des ~480 illustrations associées. Un travail réaliste pour une équipe, irréaliste pour un solo.

Le réflexe IA naïf serait : *« je demande à Claude de me générer 120 articles d'un coup, je publie, c'est fini »*. Mais ce réflexe produit deux désastres :

1. **Désastre tonal** — le modèle dérive sur des centaines d'articles, l'identité Corruption-Z se dilue, on retombe dans le générique post-apo cyberpunk-friendly que le manifeste a précisément combattu.
2. **Désastre de stockage** — 120 articles publiés d'un coup ne servent ni le SEO, ni l'expérience utilisateur, ni le rythme éditorial réel d'un site vivant.

La pipeline a été conçue pour résoudre ces deux problèmes ensemble — et plus largement, pour **garder l'humain dans la boucle de bout en bout** sans pour autant lui faire faire le travail répétitif.

### La méthode « main → doc → IA »

Plutôt que de demander à l'IA d'inventer la pipeline, l'approche a été l'inverse :

1. **Faire à la main** d'abord — pas par paresse, par discipline. C'est la seule façon de savoir vraiment ce qui se passe à chaque étape.
2. **Documenter chaque micro-étape** au moment où on la fait — y compris les bouts qui semblent triviaux (la recherche par titre, le copier-coller du slug, les onglets de terminal).
3. **Identifier les irritants au passage** — les frictions qu'on ressent en faisant le travail, pas après.
4. **Soumettre la description à l'IA** comme matière brute, pas comme spécification finie.
5. **Itérer avec l'IA** sur l'automatisation — l'IA propose, on valide ou on corrige, le process se raffine.

Ce que ça produit n'est pas juste un script. C'est **une chaîne automatisée que je comprends de bout en bout**, parce que je l'ai faite à la main avant. Si elle casse, je sais où chercher. Si elle évolue, je sais où intervenir. C'est l'inverse exact du *« j'ai demandé à ChatGPT de me faire un script et ça marche, je sais pas pourquoi »*.

### Étude de cas — l'import d'images

Le cas le plus parlant. Voici le before/after qui illustre la méthode.

#### Avant — process à la main (~10 min par article)

> 1. Dans les fichiers du projet, je recherche le titre de l'article pour lequel il manque des images. Mon but : récupérer les 3-4 prompts d'images à mettre dans Midjourney. Je remplace le titre dans la chaîne ci-dessous et je fais une recherche dessus pour trouver le fichier de l'article :
>    `title: "Elias Thorne, le Hiérophante des Gardiens de l'Éveil"`
> 2. Une fois le fichier trouvé, je scroll en bas pour récupérer le prompt de la première image (héros), je le copie, je le colle dans Midjourney avec mon image de référence, je lance la génération.
> 3. Je passe à l'image suivante, etc.
> 4. Quand toutes les images sont générées, je check le résultat dans Midjourney, je like celle que je veux (Midjourney en produit 4 par prompt), je la télécharge. Pour la première — l'image héros — je repasse sur la page de dev pour copier l'URL, le slug. Je renomme l'image dans `~/Downloads/` avec le slug et le suffixe : `-hero`, `-1`, `-2`, voire `-3`.
> 5. Dans l'instruction suivante, je remplace le slug à la main avec celui de l'article que je traite :
>    ```bash
>    ./seo-pipeline/import-images.sh \
>      elias-thorne-hierophante-gardiens-corruption-z \
>      ~/Downloads/elias-thorne-hierophante-gardiens-corruption-z-hero.png \
>      ~/Downloads/elias-thorne-hierophante-gardiens-corruption-z-1.png \
>      ~/Downloads/elias-thorne-hierophante-gardiens-corruption-z-2.png \
>      ~/Downloads/elias-thorne-hierophante-gardiens-corruption-z-3.png
>    ```
>    *Remarque : il faudrait adapter le script pour qu'il fasse quand même le traitement, parfois certains articles n'ont que deux images.*
> 6. Je lance la commande dans un onglet, après l'avoir adaptée (si 2 ou 3 images).
> 7. Si tout s'est bien passé, je relance dans un autre onglet dédié : `npm run build && npm run preview`
> 8. Je vais sur l'article en question sur la page de dev et je check les images.
> 9. Si c'est OK, je clique sur la catégorie *lore* pour voir les articles auxquels il manque des images (je vois rapidement, ils n'ont pas d'image héros).
> 10. Je trouve l'article suivant, je copie le titre depuis l'URL, et je retourne à l'étape 1.

Friction réelle : **identification manuelle du fichier**, **adaptation du script à la main pour 2/3/4 images**, **renommage répétitif**, **navigation entre onglets**, **pas de feedback automatique** sur l'avancement global.

#### Après — script unique (`mj-workflow.sh`)

```bash
./seo-pipeline/mj-workflow.sh elias-thorne-hierophante-gardiens-corruption-z
```

Ce script :

1. **Vérifie que l'article existe**, sinon suggère des slugs proches (fini la chasse au fichier)
2. **Alerte si des images existent déjà** et demande confirmation avant écrasement
3. **Affiche les prompts Midjourney** prêts à copier, dans l'ordre, sans avoir à scroller
4. **Détecte automatiquement** combien d'images sont attendues (2, 3 ou 4)
5. **Attend** que les images soient téléchargées et nommées dans `~/Downloads/`
6. **Convertit** automatiquement en `.webp` et les place dans `public/images/blog/`
7. **Lance le build** et donne l'URL pour vérifier visuellement

Temps par article : **~3 min de travail humain réel** (le reste, c'est du temps Midjourney que le script ne peut pas accélérer).

#### Ce qui a rendu le saut possible

Pas la magie de l'IA — la **précision de la doc en entrée**. Les irritants étaient déjà identifiés (« il faudrait adapter le script pour 2 ou 3 images »). L'IA n'a pas eu à *deviner* ce qui n'allait pas. Elle a eu à *implémenter* ce que j'avais déjà compris en faisant.

C'est cette mécanique — **faire pour comprendre, documenter pour préciser, déléguer pour scaler** — qui se généralise à toute la pipeline.

---

## II. Architecture finale

### Vue d'ensemble

```
┌────────────────────────────────────────────────────────────────────┐
│                         AMONT — PRÉPARATION                         │
├────────────────────────────────────────────────────────────────────┤
│                                                                    │
│   ┌─────────────────┐      ┌──────────────────┐                    │
│   │  Calendrier CSV │      │   Lore (5 docs)  │                    │
│   │  120 articles   │      │   + Manifeste    │                    │
│   │  + slug + pilier│      │   + Bestiaire    │                    │
│   └────────┬────────┘      └─────────┬────────┘                    │
│            │                         │                             │
│            └────────────┬────────────┘                             │
│                         ▼                                          │
│              ┌─────────────────────┐                               │
│              │  Slash-command      │                               │
│              │  /generate-seo-     │                               │
│              │  article            │                               │
│              └─────────┬───────────┘                               │
└────────────────────────┼───────────────────────────────────────────┘
                         │
                         ▼
┌────────────────────────────────────────────────────────────────────┐
│                  GÉNÉRATION — Claude Code en batch                  │
├────────────────────────────────────────────────────────────────────┤
│                                                                    │
│   Lots de 15-25 articles · ~30 min/lot · tracker.csv               │
│                                                                    │
│   Pour chaque article :                                            │
│   ┌──────────────────────────────────────────────────────────┐     │
│   │  Lecture lore + schéma + articles existants (1× par lot) │     │
│   │  → Identification du type narratif (GUIDE/FRAGMENT/ESSAI)│     │
│   │  → Génération frontmatter (validé contre schéma Zod)     │     │
│   │  → Rédaction selon contraintes SEO + tonales             │     │
│   │  → Prompts Midjourney pré-rédigés en pied d'article      │     │
│   └──────────────────────────────────────────────────────────┘     │
│                                                                    │
└────────────────────────────────────┬───────────────────────────────┘
                                     │
                                     ▼
┌────────────────────────────────────────────────────────────────────┐
│              IMAGES — Midjourney + script d'import                  │
├────────────────────────────────────────────────────────────────────┤
│                                                                    │
│   ./mj-workflow.sh <slug>                                          │
│   → Affiche prompts → Attend les images → Convertit en webp →      │
│   Place dans public/images/blog/ → Build de validation             │
│                                                                    │
└────────────────────────────────────┬───────────────────────────────┘
                                     │
                                     ▼
┌────────────────────────────────────────────────────────────────────┐
│              PUBLICATION — Cloudflare + filtre temporel             │
├────────────────────────────────────────────────────────────────────┤
│                                                                    │
│   Filtre : pubDate <= now()  →  les articles à venir restent       │
│                                  invisibles côté lecteur           │
│                                                                    │
│   ┌────────────────────┐         ┌──────────────────────┐          │
│   │  Worker Cloudflare │ ──cron→ │  Deploy Hook         │          │
│   │  0 8 * * 1,4       │         │  Cloudflare Pages    │          │
│   │  (lun + jeu, 8h)   │         │  → build → deploy    │          │
│   └────────────────────┘         └──────────┬───────────┘          │
│                                             │                      │
└─────────────────────────────────────────────┼──────────────────────┘
                                              │
                                              ▼
                                    ┌─────────────────┐
                                    │ corruption-z.com│
                                    └─────────────────┘
```

### Les composants clés

**Calendrier éditorial** (`seo-pipeline/calendrier_editorial_corruption_z.csv`)
120 lignes, 13 colonnes. Source unique de vérité : titre, slug, mots-clés, intention de recherche, audience, pilier, fragment associé, suggestions d'images. C'est la **spec exhaustive** lue par Claude Code à chaque génération.

**Slash-command** (`.claude/commands/generate-seo-article.md`)
Le prompt structuré en 6 étapes qui pilote la génération. Lecture du contexte → identification du type narratif → mapping pilier → génération du frontmatter conforme au schéma → rédaction selon les contraintes → mise à jour du tracker. C'est le **garde-fou tonal** qui empêche la dérive sur 120 articles.

**Schéma de validation** (`src/content.config.ts`)
Astro Content Collections + Zod. Chaque article est validé contre un schéma typé au moment du build : si Claude Code livre un article avec un champ manquant, une catégorie hors enum, ou un type incompatible, **le build échoue** — pas la peine d'attendre la production pour découvrir le problème.

**Filtre temporel** (`pubDate <= now()`)
Appliqué sur 4 fichiers de listing. Permet de pousser 120 articles d'un coup tout en n'en affichant que ceux dont la date est arrivée. Stratégie SEO « bibliothèque + rythme » : 15 articles fondateurs publiés au lancement, 105 étalés sur ~50 semaines.

**Worker + Cron Trigger** (`0 8 * * 1,4`)
Le mécanisme de publication automatique. Tous les lundis et jeudis à 8h UTC, le Worker appelle le Deploy Hook de Cloudflare Pages, qui rebuild le site et publie les nouveaux articles dont la `pubDate` est désormais passée. Aucune intervention humaine requise une fois la machine lancée.

### Chiffres réels

| Indicateur | Valeur |
|---|---|
| Articles planifiés | 120 |
| Articles générés (à ce jour) | 4 (semaines 1-4) + 5 legacy |
| Lots de génération | 6 lots de 15-25 articles |
| Durée d'un lot | ~30 minutes |
| Prompts Midjourney pré-rédigés | ~480 |
| Builds par semaine | 2 (lundi + jeudi) |
| Période de publication | mai 2026 → avril 2028 (≈ 2 ans) |

---

## III. Aperçu du flow concret

Cinq étapes dans la vie réelle de la pipeline, du calendrier au site en ligne :

1. **Calendrier** — un CSV de 120 lignes décrit chaque article (titre, slug, mots-clés, intention de recherche, audience, pilier, fragment associé). C'est la source unique de vérité.

2. **Génération** — Claude Code lit le calendrier, le lore et le schéma une fois par lot, puis produit les articles `.md` un par un selon la slash-command `/generate-seo-article`. Lots de 15 à 25 articles, ~30 minutes par lot, branche dédiée `auto/seo-batch-corruption-z`.

3. **Images** — pour chaque article publié, le script `mj-workflow.sh` affiche les prompts Midjourney pré-rédigés, attend les fichiers téléchargés avec un nommage convenu, les convertit en `.webp` et les place dans le dossier d'images.

4. **Build & déploiement** — push sur `main`, Cloudflare Pages détecte, build, déploie. Aucune commande supplémentaire.

5. **Publication différée** — le filtre `pubDate <= now()` masque les articles dont la date n'est pas arrivée. Le Worker Cloudflare rebuild le site tous les lundis et jeudis à 8h UTC, ce qui rend visibles les articles dont la date vient d'être atteinte.

**Flow type sur une semaine pour moi :** choisir 3-5 articles à illustrer parmi ceux qui n'ont pas encore d'image hero, lancer `mj-workflow.sh` pour chacun, copier les prompts dans Midjourney, valider visuellement, télécharger, laisser le script importer et builder, commit & push. Cloudflare déploie en 2-3 minutes, le cron du mardi/jeudi prend le relais pour publier les articles dont la `pubDate` arrive à échéance.

→ **Mode d'emploi complet : [06-pipeline-mode-emploi.md](./06-pipeline-mode-emploi.md)** — pré-requis, prompt batch pour Claude Code, lots recommandés, conventions de nommage, annexes de vérification, dépannage.

---

← [Retour à l'index](./README.md) · [Précédent : Manifeste ←](./01-manifeste.md) · [Suite : Lore →](./02-lore.md)

---

*Dernière mise à jour : 8 mai 2026*
