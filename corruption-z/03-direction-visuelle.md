# 03 — Direction visuelle

← [Retour à l'index](./README.md)

---

## I. Les fondations

La direction visuelle de Corruption-Z tient en quatre matériaux : **pierre humide, or terni, rouge sang séché, noir profond**. Aucun chrome, aucun néon, aucun écran lumineux. Le monde est minéral, organique, ancien — ses textures sont celles du XIIe siècle balkanique, pas celles d'une corporation high-tech. Tout ce qui est techno-futuriste est un anti-pattern absolu.

Le syncrétisme religieux est délibérément déformé : christianisme orthodoxe + apocryphes + gnosticisme + paganisme slave + énochien, mélangés en proportions suffisantes pour qu'aucune ne domine. Le joueur doit reconnaître les symboles sans les replacer dans une religion réelle.

**Inspirations majeures :** Bloodborne (gothique sacré corrompu), Berserk (brutalité tragique), Hellblade (psychose et syncrétisme), Silent Hill 2-3 (oppression sonore et symbolisme psycho-spirituel), The Witch (folk horror chrétien-puritain), Control (anomalies de réalité), Annihilation (corruption qui transforme la matière).

**Inspirations en peinture :** Caravage (tenebrism, lumière unique sur fond noir), Beksinski (corps émaciés, paysages oniriques toxiques), Andrew Wyeth (mélancolie minérale rurale), Caspar David Friedrich (sublime romantique tragique), Goya (peintures noires), Adrian Ghenie (matière picturale moderne, distorsion).

---

## II. La méthode Midjourney

### Le principe : `--sref` pour la cohérence

Toutes les illustrations du projet partagent une même **référence stylistique** (`--sref`) qui garantit qu'elles paraissent issues du même univers visuel, peintes par la même main. C'est la clé qui empêche la dérive d'un visuel à l'autre. Sans cette référence partagée, MJ peut faire un Marcheur en oil painting puis un Hurleur en aquarelle puis un Tisseur en cinematic render — ce serait un patchwork incohérent. Avec, on a une bibliothèque visuelle unifiée.

### La structure de prompt type

Chaque prompt suit une **architecture fixe** :

1. **Sujet** (ce qu'on veut voir, en premier — l'ordre compte)
2. **Cadrage** (taille du sujet dans le frame, position, mouvement)
3. **Décor / contexte** (ce qui entoure le sujet, en second plan)
4. **Ambiance** (le ressenti visé, l'émotion)
5. **Palette** (couleurs autorisées, couleurs explicitement bannies)
6. **Médium** (oil painting, brushstrokes, impasto)
7. **Références peintres** (Caravage meets Beksinski meets...)
8. **Garde-fous** `--no` (anti-cyberpunk, anti-fantasy, anti-blockbuster — et **spécifiques au sujet**)

### Un exemple concret — un prompt complet

Pour donner corps à cette architecture, voici un prompt utilisé en production. Il illustre une « toile abandonnée » — une image de bestiaire montrant la trace d'un Tisseur sans le Tisseur lui-même.

```
A wide 3:2 dark atmospheric oil painting with gritty brushstrokes and
heavy impasto texture, an extreme close-up macro view of an old dusty
spider web of dirty white silk threads stretched between rough damp
stone blocks in a narrow underground corridor wall, the silk threads
form an intricate radial geometric pattern reminiscent of orthodox
religious ornament tracery, the silk is dirty bone white slightly
yellowed with age and dust, no luminescence no glow, the threads catch
only the diffuse ambient cold light from the corridor, dust particles
visible on the silk, the wet stone surface has moisture droplets that
catch faint light, a small distant guttering candle visible at the far
end of the corridor through the threads its warm tarnished gold flame
the only colored light in the scene, the floor visible between threads
shows faint disturbed marks in the dust suggesting recent passage. No
figures anywhere only the trace of presence. The ambiance is moody,
oppressive, contemplative, palette dominated by deep umber, slate grey,
dusty bone parchment beige silk threads, faint warm tarnished gold from
the distant candle, dark mossy green moisture, no blue tones, slavic
gothic horror melancholy. Painterly visible brushwork thick paint
texture, deep Caravaggio extreme tenebrism with the distant candle as
the only warm point of light. Caravaggio meets Beksinski meets Adrian
Ghenie meets Goya black paintings, the territorial markings of an
unseen patient predator, no glamour no jump-scare
--ar 3:2 --style raw --s 220
--no photoreal, photograph, sharp digital, smooth, anime, vibrant, neon,
cgi, 3d render, fantasy environment, video game asset, blue, blue tones,
cyan, teal, navy, indigo, cyanotype, monochrome blue, sci-fi, cyberpunk,
action scene, visible spider, hairy spider, jewelry decoration,
halloween decoration, fairy lights, christmas lights, glowing threads,
luminescent threads, magical glow, incandescent, fire light threads,
lava threads, hot orange threads, electrical light, neural network,
light emanating from threads, gothic romantic
```

Trois choses à remarquer dans ce prompt :

- **Les répétitions explicites de la négation.** « No luminescence no glow » dans le corps du prompt + 6 termes anti-glow dans le `--no` (`glowing threads, luminescent threads, magical glow, light emanating from threads, electrical light, fairy lights`). Cette redondance est volontaire : MJ a une tendance forte à faire « briller » les fils de toile par défaut. Une seule mention ne suffit pas.

- **Les `--no` spécifiques au sujet.** Au-delà des anti-patterns transverses du projet (cyberpunk, sci-fi, action scene), ce prompt cible les pièges du sujet « toile d'araignée » : les décorations de Halloween, les fairy lights, le « gothic romantic » (genre Tim Burton joli), la lumière émanant des fils. Chaque catégorie de sujet a ses propres pièges, identifiés à l'usage.

- **L'ancrage de la lumière.** « The distant candle as the only warm point of light » impose une source de lumière unique. Sans cette précision, MJ disperse l'éclairage et tue la composition tenebrist.

### Les leçons distillées par l'expérience

Quatre principes cardinaux, appris à travers des sessions concrètes (dont celle qui suit) :

1. **L'ordre du prompt = la hiérarchie visuelle.** Ce qui vient en premier domine l'image. Si le sujet doit être central, il doit ouvrir le prompt — pas le décor.

2. **La métaphore est prise au littéral.** Quand on écrit *« moves like an inverted arachnid »*, MJ ne lit pas la métaphore — il lit l'instruction. Pour suggérer une posture sans l'incarner littéralement, il faut désigner une **référence reconnaissable** (« Linda Blair spider walk ») plutôt que décrire mécaniquement (« inverted arachnid »).

3. **Les répétitions explicites bloquent les dérives.** Si MJ tend vers une interprétation indésirable, mentionner le contre 10-12 fois fonctionne. C'est inélégant linguistiquement, c'est efficace techniquement.

4. **Les variations sont sous-utilisées.** Quand un prompt produit 1-2 résultats sur 4 qui sont proches du but, les variations Midjourney (sur la meilleure image) explorent l'espace voisin et produisent souvent le résultat final, mieux qu'une réécriture du prompt qui repart de zéro.

---

## III. Étude de cas — le HERO du Tisseur

> *Cette section documente une session réelle de production : 7 itérations de prompt, ~28 images générées, pour aboutir à une seule illustration validée. Elle est conservée dans son intégralité, échecs compris, parce que les échecs racontent la méthode mieux que les succès.*

### Le sujet — pourquoi c'est compliqué

Le **Tisseur** est l'un des damnés du bestiaire. Ancien chasseur dont l'âme s'est dissoute lentement, il occupe désormais les hauteurs des cathédrales en ruines, accroché au plafond, descendant en spider walk pour atteindre ses proies. Son anatomie reste humaine — pas d'arachnide ajouté — mais ses articulations sont **inversées** : il se déplace à quatre pattes en posture de pont cambrée, ventre vers le ciel, tête à l'envers entre les bras.

C'est la posture iconique de la spider walk de l'Exorciste.

### La référence visuelle

Voici l'image qui a servi de référence mentale en écrivant le prompt :

![Photo de référence — Linda Blair spider walk](./images/etude-de-cas-prompt-midjourney/linda-blair-reference.webp)

*La spider walk — posture humaine impossible la plus iconique du cinéma horror. Pont inversé : ventre vers le ciel, dos cambré vers le sol, tête pendante vers l'arrière, mains et pieds en appui formant les quatre points de support. C'est ça que je voulais reproduire dans l'esthétique Corruption-Z.*

### Le prompt d'origine

```
A wide cinematic 16:9 dark atmospheric oil painting with gritty
brushstrokes and heavy impasto texture, a massive creature clinging
to the upper corner of a ruined orthodox cathedral interior, the
creature is a former human with all four limbs reversed at the
joints so it moves like an inverted arachnid, its human face is
still recognizable and fixed in an expression of calm focused
concentration, looking directly downward toward the viewer, its back
covered in glandular orifices secreting faint luminescent threads
that hang down like distorted cobwebs catching what little cold
light enters through the collapsed stone roof, the cathedral nave is
visible below in deep shadow, broken pews, scattered wax pools on
the cracked stone floor, the creature's silhouette framed against a
pale grey sky visible through a gaping hole in the vaulted ceiling.
[...palette, médium, références...]
--ar 16:9 --style raw --s 220
--no photoreal, photograph, sharp digital, [...anti-patterns...]
```

Un prompt riche, soigné, qui décrit précisément la créature. Et qui va spectaculairement échouer.

---

### Itération 0 — le prompt brut

<table>
<tr>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter0-1.png" alt="Itération 0 - quadrant 1" /></td>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter0-2.png" alt="Itération 0 - quadrant 2" /></td>
</tr>
<tr>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter0-3.png" alt="Itération 0 - quadrant 3" /></td>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter0-4.png" alt="Itération 0 - quadrant 4" /></td>
</tr>
</table>

**Diagnostic :** un colosse minéral, un humain qui saute, un géant musculeux flottant, une créature reptilienne. Aucune n'est un Tisseur. **MJ a comblé le vague avec des clichés fantasy** — confronté à une description complexe avec quatre instructions corporelles simultanées (humain + araignée + inversion + filaments), il a moyenné et produit des créatures conventionnelles.

**Leçon :** quand le prompt est diffus ou trop chargé, MJ cherche le motif le plus proche dans son entraînement — donc le plus générique.

---

### Itération 1 — la simplification

> Stratégie : *« la créature est petite et dans l'ombre. On ne voit qu'une silhouette inversée, on ne cherche pas à détailler son anatomie. C'est l'ambiance qui porte l'image. »*

<table>
<tr>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter1-1.png" alt="Itération 1 - quadrant 1" /></td>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter1-2.png" alt="Itération 1 - quadrant 2" /></td>
</tr>
<tr>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter1-3.png" alt="Itération 1 - quadrant 3" /></td>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter1-4.png" alt="Itération 1 - quadrant 4" /></td>
</tr>
</table>

**Diagnostic :** quatre humains suspendus dans des cathédrales en ruines. Un grimpe une corde, un est accroché à une chaîne, deux pendent. **Aucun n'est en posture spider walk.** En simplifiant pour rendre la créature petite et suggérée, le prompt a aussi perdu la signature anatomique qui fait du Tisseur un Tisseur. MJ a retenu « humain suspendu », pas « humain inversé en posture impossible ».

**Leçon :** le piège de la simplification — quand on rend un sujet petit pour le simplifier, on perd ce qui le rend reconnaissable. La sécurité technique a coûté la signature narrative.

---

### Itération 2 — la créature cachée

> Stratégie : *« cathédrale dominante, créature 'barely visible in the shadow' »*

<table>
<tr>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter2-1.png" alt="Itération 2 - quadrant 1" /></td>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter2-2.png" alt="Itération 2 - quadrant 2" /></td>
</tr>
<tr>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter2-3.png" alt="Itération 2 - quadrant 3" /></td>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter2-4.png" alt="Itération 2 - quadrant 4" /></td>
</tr>
</table>

**Diagnostic :** quatre cathédrales magnifiquement peintes. **Aucune créature.** Pas même une silhouette dans l'ombre. MJ a interprété « barely visible » comme « ne la peins pas du tout », et a optimisé pour le sujet dominant — l'architecture, qui occupait la majorité du prompt.

**Leçon :** quand on dit à MJ de cacher quelque chose, il l'omet complètement. Il n'y a pas de juste milieu entre « présent » et « absent » — il faut choisir.

---

### Itération 3 — le sujet en premier

> Stratégie : *« créature en sujet premier, "spider walk" explicite, instruction de taille — le sujet doit dominer la composition »*

<table>
<tr>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter3-1.png" alt="Itération 3 - quadrant 1" /></td>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter3-2.png" alt="Itération 3 - quadrant 2" /></td>
</tr>
<tr>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter3-3.png" alt="Itération 3 - quadrant 3" /></td>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter3-4.png" alt="Itération 3 - quadrant 4" /></td>
</tr>
</table>

**Diagnostic :** quatre humains hybridés avec des **pattes d'araignée géantes** sortant du dos ou des épaules. La sur-correction a basculé vers l'autre extrême : MJ a pris au pied de la lettre « spider walk », « inverted arachnid », « four limbs reversed », et a littéralement ajouté des pattes d'araignée au corps humain.

**Leçon :** la métaphore est prise au littéral. MJ ne distingue pas « bouge comme une araignée » de « est une araignée ». Pour suggérer une posture sans l'incarner anatomiquement, il faut désigner une référence (« Linda Blair spider walk ») plutôt que la décrire mécaniquement.

---

### Itération 4 — le rééquilibrage anti-arachnide

> Stratégie : *« supprimer "spider", supprimer les filaments dans le dos, mentionner "human" 12 fois, citer Lucian Freud et Jenny Saville comme référents du corps humain »*

<table>
<tr>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter4-1.png" alt="Itération 4 - quadrant 1" /></td>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter4-2.png" alt="Itération 4 - quadrant 2" /></td>
</tr>
<tr>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter4-3.png" alt="Itération 4 - quadrant 3" /></td>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter4-4.png" alt="Itération 4 - quadrant 4" /></td>
</tr>
</table>

**Diagnostic :** quatre humains à quatre pattes, anatomiquement humains, vêtements déchirés, expression hostile. Pas d'arachnide. Mais **pas d'inversion non plus** — ce sont des humains qui rampent normalement, ventre vers le sol, dos vers le ciel. La sur-correction a éliminé le défaut (l'arachnide) **et la signature** (l'inversion).

**Le résultat est inquiétant dans un sens involontaire : les images sont belles.** Bien composées, bien éclairées, expression hostile, contexte gothique. Quelqu'un qui ne connaît pas le sujet pourrait dire « parfait, c'est ça le Tisseur ! ». Mais il s'agirait d'un Tisseur édulcoré — un humain agressif et émacié, pas un humain qui défie l'anatomie et la gravité.

**Leçon :** le risque de la sur-correction propre. Quand on supprime un mot dangereux (« spider »), on supprime aussi ce qu'il convoyait d'utile (l'inversion iconique). La vraie maîtrise n'est pas de **supprimer le mot dangereux**, c'est d'**isoler son sens** en désactivant ses associations parasites.

---

### Itération 5 — la bascule

> Stratégie : *« réintégrer "exorcist spider walk" comme référence cinéma, ajouter description anatomique inversée explicite (belly facing the sky, back arched downward, head hanging upside down between shoulders), ajouter un détail de preuve (ribs visible on the upturned belly), bombarder le `--no` avec les postures concurrentes (downward dog, forward bend, plank...) »*

<table>
<tr>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter5-1.png" alt="Itération 5 - quadrant 1 (proche du but)" /><br/><strong>5.1 — proche du but</strong></td>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter5-2.png" alt="Itération 5 - quadrant 2" /><br/>5.2</td>
</tr>
<tr>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter5-3.png" alt="Itération 5 - quadrant 3 (proche du but)" /><br/><strong>5.3 — proche du but</strong></td>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter5-4.png" alt="Itération 5 - quadrant 4" /><br/>5.4</td>
</tr>
</table>

**Diagnostic :** pour la première fois, **les quatre images sont sur la cible** — humains en posture inversée, tête vers le bas, mains et pieds en appui, anatomie humaine intacte, pas d'arachnide, palette earth-tones respectée. **5.1 et 5.3 sont les meilleures :** visage clairement à l'envers, cheveux qui tombent vers le sol, sensation de mouvement, dos cambré.

C'est le **moment de bascule** de toute la session. Six itérations pour arriver ici.

**Leçon :** le bon prompt produit un **spectre de candidats viables**, pas une seule réussite. Identifier les meilleurs candidats — au pluriel — devient l'enjeu.

---

### Itération 6 — l'oscillation

> Stratégie : *« précision gymnastique : "full backbend bridge gymnastic position", "chest pushed upward", "back of the head below shoulders", "inverted torso forms an arch" — pour forcer la posture exacte »*

<table>
<tr>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter6-1.png" alt="Itération 6 - quadrant 1" /><br/>downward dog yoga</td>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter6-2.png" alt="Itération 6 - quadrant 2" /><br/>vrai pont, mais palette bleue</td>
</tr>
<tr>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter6-3.png" alt="Itération 6 - quadrant 3" /><br/>visage inversé, mais tronqué</td>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter6-4.png" alt="Itération 6 - quadrant 4" /><br/>retour au rampement normal</td>
</tr>
</table>

**Diagnostic :** la cohérence éclate. Quatre images **radicalement différentes**, dont deux régressent (le yoga downward dog que le `--no` bombardait précisément, et le rampement normal de l'itération 4). Une bascule dans le bleu, contre la palette du prompt. Une montre un visage à l'envers saisissant, mais tronqué par le haut.

C'est l'**oscillation typique** : on a corrigé trop fort dans une direction, on a déstabilisé ce qui marchait. La précision technique excessive a rouvert l'espace de recherche au lieu de le resserrer.

**Leçon :** trop préciser, c'est rouvrir l'espace de recherche. Quand un prompt fonctionne (comme à l'itération 5), pousser plus loin la précision peut l'abîmer. **Une fois qu'on a un spectre de bons candidats, on arrête de réécrire le prompt — on bascule sur les variations.**

---

### Itération 7 — la résolution par variations

> Stratégie : *« on arrête de réécrire. On lance des variations Midjourney sur les meilleures images de l'itération 5 (5.1 et 5.3). MJ reste dans la zone qui marche et explore les variantes proches. »*

<table>
<tr>
<td colspan="2"><img src="./images/etude-de-cas-prompt-midjourney/tisseur-hero-final.png" alt="HERO final validé" /><br/><strong>HERO final — validé pour le bestiaire</strong></td>
</tr>
<tr>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter7-variation-2.png" alt="Variation 2" /><br/>Variation 2</td>
<td><img src="./images/etude-de-cas-prompt-midjourney/tisseur-iter7-variation-3.png" alt="Variation 3" /><br/>Variation 3</td>
</tr>
</table>

**Diagnostic :** les variations sur 5.1 et 5.3 ont produit **trois images viables, dont une parfaitement aboutie**. Le HERO final présente :

- Posture spider walk inversée correctement rendue (tête en bas, mains et pieds en appui)
- Visage humain reconnaissable, expression calme et prédatrice — pas d'hystérie démoniaque
- Anatomie humaine intacte, pas d'arachnide
- Vêtements en lambeaux, mains crispées sur la pierre, bandeau au front
- Palette earth-tones respectée (ash, umber, bone white, no blue)
- Brushwork visible, tenebrism Caravage
- Composition cinématographique 16:9 propre

**Leçon centrale, et la plus contre-intuitive :** quand un prompt produit plusieurs candidats viables, **les variations sur ces candidats produisent souvent le résultat final, mieux qu'une réécriture qui repart de zéro**. Cette règle est sous-utilisée par la majorité des utilisateurs Midjourney qui réécrivent compulsivement leur prompt.

---

### Auto-critique honnête de Claude (en fin de session)

Après le succès, Claude lui-même a tenu à pointer ce qui aurait pu être fait mieux :

> *« Pour une seule image hero, on a passé énormément de temps. Et ce n'est pas exclusivement de la responsabilité de MJ — c'est aussi un signal sur ma manière de t'aider qui mérite réflexion.*
>
> *Ce qui s'est passé :*
> - *J'ai sur-itéré au lieu de t'orienter plus tôt vers la solution la plus rapide.*
> - *J'ai oscillé (corriger trop dans une direction, puis trop dans l'autre).*
> - *J'aurais dû te suggérer dès l'itération 3 ou 4 de passer en `--cref` ou `--sref` avec la photo de référence Linda Blair que tu avais fournie.*
> - *J'aurais dû te dire plus tôt que pour des postures aussi spécifiques, le prompt textuel atteint vite ses limites.*
>
> *Je te dois aussi une excuse honnête : j'aurais dû te proposer `--cref` dès l'itération 3 au lieu de continuer à réécrire des prompts textuels. C'est ma responsabilité de te le suggérer, pas la tienne de me le rappeler. »*

Cette auto-critique est **précieuse** parce qu'elle ouvre une réflexion qui dépasse la session : **certaines choses se montrent au lieu de se décrire**. Quand on a une référence visuelle précise sous la main, l'uploader directement à MJ via `--cref` (image de pose) ou `--sref` (référence stylistique) est presque toujours plus rapide et plus précis qu'une description textuelle élaborée.

Mais — et c'est la nuance qui rend la conclusion plus juste — la méthode laborieuse a aussi un bénéfice. Le HERO final n'est **pas une copie de la photo Linda Blair** : il transpose la posture iconique dans l'esthétique Corruption-Z (peinture huile, palette earth-tones, visage prédateur calme et non hystérique, vêtements en lambeaux, contexte gothique). C'est une **traduction culturelle**, pas un décalque. Un `--cref` direct aurait sans doute gardé trop de marqueurs « film 2010, fond carrelage, sang sur le visage ». La méthode laborieuse a permis d'isoler la posture et de la rejouer dans l'univers du jeu.

L'auto-critique est valable. Elle est aussi partielle.

---

### Récapitulatif visuel

| Itération | Stratégie | Résultat | Leçon |
|---|---|---|---|
| **0** | Prompt brut | Créatures fantasy aléatoires | Le vague produit des clichés |
| **1** | Créature suggérée petite | Humain qui grimpe | Discrétion = perte de signature |
| **2** | Créature « barely visible » | Cathédrales vides | « Barely visible » = absent |
| **3** | Sujet premier + spider walk | Hybrides homme-araignée | La métaphore prise au littéral |
| **4** | Anti-arachnide, « human » × 12 | Humains qui rampent | Sur-correction = signature effacée |
| **5** | Description anatomique inversée | **Plusieurs candidats viables** | Le bon prompt produit un spectre |
| **6** | Précision gymnastique | Dispersion incohérente | Trop préciser = rouvrir la recherche |
| **7** | **Variations sur 5.1 et 5.3** | **HERO final + 2 variantes** | Variations > réécriture |

### Grille décisionnelle distillée

| Situation | Stratégie |
|---|---|
| 1ère génération donne quelque chose de proche mais imparfait | **Variations + reroll** |
| 2ème prompt textuel n'améliore pas | **Passer en `--cref` ou `--sref`** avec image de référence |
| 3ème prompt n'améliore toujours pas | **Image-to-image** ou accepter le moins pire |
| 4ème échec consécutif | **Repartir d'un autre angle complètement** |

**Plus de 4 itérations sur le même sujet = signal d'arrêt.** Au-delà, MJ tourne en rond et l'opérateur aussi.

---

## IV. Pour les autres damnés

Le Tisseur est le cas le plus dur que la production a traité. Pour les autres damnés du bestiaire — **Marcheur, Hurleur, Pater Vox, Aranea Maledicta, Custos Carnis, Vestigium Animae** — la méthode (`--sref` partagée + structure de prompt + garde-fous explicites) a marqué du premier coup, ou en 1-2 itérations. C'est pourquoi cette page documente le Tisseur en détail : c'est précisément parce qu'il est l'exception qu'il enseigne le plus.

Le bestiaire visuel complet est en cours de production, au rythme du calendrier éditorial — chaque damné illustré est ensuite intégré aux articles qui le concernent sur [corruption-z.com](https://corruption-z.com).

---

← [Retour à l'index](./README.md) · [Précédent : Lore ←](./02-lore.md) · [Suite : Direction musicale →](./04-direction-musicale.md)