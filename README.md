# TP1 — Kit d'interface animé

**Animation Web · L3 Informatique · ITUniversity**
**Séance 1 — Transitions et transformations CSS · Durée : 1 h 20**

---

## Objectifs

À la fin de ce TP, vous saurez :

1. déclencher un changement d'état avec une pseudo-classe (`:hover`, `:focus`, `:active`, `:focus-visible`) ;
2. rendre ce changement progressif avec `transition` ;
3. utiliser `transform` (`translate`, `scale`, `rotateY`) sans casser la mise en page ;
4. vérifier qu'une animation fonctionne aussi **au clavier**.

## Démarrage

1. Copiez le dossier `starter/` dans votre dépôt Git, sous le nom `seance1/`.
2. Ouvrez `seance1/index.html` dans votre navigateur, et `style.css` dans votre éditeur.
3. **Vous ne modifiez que `style.css`**, et uniquement aux endroits marqués `TODO`. Le HTML est déjà prêt.

Gardez les DevTools ouverts (F12) : l'onglet *Elements* permet de forcer un état `:hover` ou `:focus` (bouton `:hov`), très pratique pour régler une animation.

| Niveau | Contenu | Durée indicative |
|---|---|---|
| 1 | Boutons | 25 min |
| 2 | Formulaire | 25 min |
| 3 | Cartes | 20 min |
| Bonus | Carte qui se retourne en 3D | 10 min |

---

## Niveau 1 — Boutons

### Étape 1.1 — Observer le problème

Survolez les trois boutons. Leur couleur change déjà (c'est dans le code fourni), mais **d'un coup**.

> **Question 1.** Quelle ligne du CSS provoque ce changement ? Pourquoi est-il instantané ?

### Étape 1.2 — Rendre le changement progressif

Dans `.btn`, ajoutez une transition sur `background-color`, d'une durée de `0.2s`, avec la fonction `ease`.

Vérifiez : le changement de couleur est-il maintenant progressif **à l'aller ET au retour** ?

### Étape 1.3 — Soulever le bouton

Dans `.btn:hover`, faites monter le bouton de 2 px avec `transform: translateY(...)`.

Le bouton saute d'un coup ? C'est normal : votre transition ne concerne que `background-color`. Complétez-la pour animer aussi `transform` (durée `0.15s`, fonction `ease-out`).

> 💡 Plusieurs propriétés dans une même transition se séparent par des **virgules**.

### Étape 1.4 — Effet « enfoncé » au clic

Ajoutez une règle `.btn:active` qui ramène le bouton à sa position (`translateY(0)`) et le réduit légèrement (`scale(.97)`).

> 💡 Les deux fonctions s'écrivent dans **une seule** propriété `transform`, séparées par un espace.

### Étape 1.5 — Ne pas oublier le clavier

Appuyez sur **Tab** pour naviguer entre les boutons. Voyez-vous où vous êtes ?

Ajoutez une règle `.btn:focus-visible` avec un contour vert : `outline: 3px solid var(--green);` et un décalage `outline-offset: 3px;`.

### Étape 1.6 — Expérience : le piège du placement

1. **Déplacez** temporairement votre ligne `transition` de `.btn` vers `.btn:hover`.
2. Survolez un bouton, puis quittez-le.

> **Question 2.** Que se passe-t-il au retour ? Expliquez pourquoi.

3. Remettez la transition à sa place, dans `.btn`.

✅ **Checkpoint niveau 1**
- [ ] La couleur change en douceur, à l'aller et au retour
- [ ] Le bouton monte au survol et s'enfonce au clic
- [ ] Le focus clavier est visible
- [ ] Question 1 et question 2 répondues (en commentaire dans `style.css`)

---

## Niveau 2 — Formulaire

### Étape 2.1 — Mettre en valeur le champ actif

Créez une règle `.field input:focus` qui :
- passe la bordure en vert (`border-color: var(--green)`) ;
- ajoute un halo : `box-shadow: 0 0 0 4px rgba(118, 183, 41, .2);`

### Étape 2.2 — Adoucir le changement

Dans `.field input`, ajoutez une transition sur `border-color` **et** `box-shadow` (0.2s).

### Étape 2.3 — Le label flottant

Objectif : quand on clique dans le champ, le label « Nom complet » **monte et rétrécit** pour laisser la place au texte, puis **reste en haut** si le champ est rempli.

1. Dans `.field label`, définissez le point d'appui en haut à gauche (`transform-origin: left top;`) et une transition sur `transform` et `color` (0.2s, `ease-out`).
2. Écrivez une règle qui s'applique au label **quand l'input a le focus** :
   `.field input:focus + label { ... }`
   avec `transform: translateY(-.65rem) scale(.8);` et `color: var(--navy);`
3. Testez : tapez un nom, puis cliquez ailleurs. Le label redescend sur votre texte ! Il faut aussi le garder en haut quand le champ **contient du texte**.

> 💡 Indice : le HTML contient `placeholder=" "`. Cherchez ce que signifie la pseudo-classe `:placeholder-shown`, et comment l'inverser avec `:not(...)`. Ajoutez ce second sélecteur à la même règle, séparé par une virgule.

> **Question 3.** Pourquoi le label est-il placé **après** l'input dans le HTML ?

> **Question 4.** Pourquoi utiliser `transform` plutôt que modifier `top` pour faire monter le label ?

✅ **Checkpoint niveau 2**
- [ ] Bordure et halo apparaissent en douceur au focus
- [ ] Le label monte au focus et reste en haut si le champ est rempli
- [ ] Le label redescend si on vide le champ
- [ ] Questions 3 et 4 répondues

---

## Niveau 3 — Cartes

### Étape 3.1 — Élévation au survol

Au survol d'une carte (`.card:hover`) :
- elle monte de 6 px ;
- son ombre s'agrandit : `box-shadow: 0 12px 24px rgba(12, 16, 31, .15);`

N'oubliez pas la transition dans `.card` (0.25s, `ease-out`).

### Étape 3.2 — Zoom sur le visuel

Au survol de la **carte** (et non du cercle seul), le cercle vert `.card__visual` grossit à `scale(1.15)`.

> 💡 Sélecteur descendant : « le visuel qui se trouve dans une carte survolée ».

### Étape 3.3 — Observer

Pendant le survol, les cartes voisines bougent-elles ? La grille se décale-t-elle ?

> **Question 5.** Refaites l'étape 3.1 en remplaçant `transform` par `margin-top: -6px`. Que constatez-vous ? Quelle conclusion en tirez-vous ?

(Remettez ensuite `transform`.)

✅ **Checkpoint niveau 3**
- [ ] Les cartes s'élèvent en douceur sans décaler leurs voisines
- [ ] Le visuel grossit au survol de la carte entière
- [ ] Question 5 répondue

---

## Bonus — Carte qui se retourne en 3D

Pour l'instant, la face « Réponse » recouvre la face « Question ». Suivez les `TODO B.1` à `B.5` :

1. **B.1** — Sur `.flip` (le parent) : `perspective: 1000px;`
2. **B.2** — Sur `.flip__inner` : une transition sur `transform` (0.6s) et `transform-style: preserve-3d;`
3. **B.3** — Au survol **ou au focus** de `.flip`, faire pivoter `.flip__inner` de 180° autour de l'axe Y.
4. **B.4** — Sur `.flip__face` : `backface-visibility: hidden;` (une face vue de dos devient invisible).
5. **B.5** — La face arrière part déjà retournée : `transform: rotateY(180deg);`

Testez à la souris, puis **au clavier** (Tab jusqu'à la carte).

> **Question 6.** Retirez `perspective`. Que devient l'effet ?

---

## Livrable

- Dossier `seance1/` dans votre dépôt Git, avec `index.html` et `style.css` complétés.
- Réponses aux questions 1 à 6 **en commentaire** en haut de `style.css`.
- Commit et push en fin de séance :

```bash
git add seance1/
git commit -m "Séance 1 : transitions et transformations"
git push
```

## Critères d'évaluation (10 % de la note du module)

| Critère | Points |
|---|---|
| Niveau 1 fonctionnel (y compris focus clavier) | 3 |
| Niveau 2 fonctionnel (label flottant complet) | 3 |
| Niveau 3 fonctionnel (sans décalage des voisins) | 2 |
| Réponses aux questions, justes et argumentées | 2 |
| *Bonus : carte 3D fonctionnelle à la souris et au clavier* | *+1* |
| **Total** | **10** |

## Pour aller plus loin

- MDN — Utiliser les transitions CSS : <https://developer.mozilla.org/fr/docs/Web/CSS/CSS_transitions/Using_CSS_transitions>
- MDN — `transform` : <https://developer.mozilla.org/fr/docs/Web/CSS/transform>
- MDN — `:placeholder-shown` : <https://developer.mozilla.org/fr/docs/Web/CSS/:placeholder-shown>
