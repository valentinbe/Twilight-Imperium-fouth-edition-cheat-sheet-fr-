# Twilight Imperium fouth edition cheat sheet (fr)

## Ordre du tour

1. Phase de stratégie
2. Phase d'action
3. Phase de statut
4. Phase de projet

## 1. Phase de stratégie

Draft une carte, sens horaire en commencant par l'orateur, puis mettre 1 bien commercial sur les cartes restantes

### Explication des cartes

- **1 Gouvernance\***: Gagner plus de jetons d'actions (+3 pions commandement)
- **2 Diplomatie**: Se protéger des attaques ennemies + restaurer ressources du système
- **3 Politique\***: Pioche cartes actions et pouvoirs dans décisions politiques (choix orateur + scry 2 paquet projet)
- **4 Construction\***: Pour construire des structures sur ses planètes (pour défense et prod d'unités)
- **5 Commerce\***: Faire du commerce et gagner des ressources (Biens commerciaux + commodités)
- **6 Guerre\***: Se déplacer deux fois avec un vaisseau ou se déplacer et produire sur un même système en un round + modifier sa stratégie en cours de round
- **7 Technologie\***: Recherche de technologies
- **8 Intrigue**: Scorer des points de victiore + piocher objectif secret

_\* si bon choix en début de partie_

## 2. Phase d'action

Tourne en fonction de l'initiative des cartes stratégie
A chaque tour, choix de l'une des trois actions suivantes, puis joueur suivant:

- **Stratégique**:
  - Effectuer l'action primaire de sa carte stratégie,
  - Chaque adversaire peut faire son action secondaire, joueur gauche, sens horaire.
  - Retourner la carte stratégie
- **Element de jeu**: Carte avec mot clé **Action**
  - _Pas possible de cible plusieurs fois la meme cible avec plusieurs cartes action ayant le même nom_
- **Tactique**: CF partie suivante
- **Passer**: `if(carte stratégie retournée)` Ne peut plus effectuer d'action pour le round sauf actions secondaires des cartes stratégiques
  - Possible de jouer des cartes action (combat tricks) après avoir passé
  - Possible de jouer les actions secondaires des cartes stratégiques après avoir passé

### Action tactique

- Placer un pion Commandement dans un système
- Déplacer ses vaisseaux dans le système
  - Toute unité à portée peuvent se déplacer simultanément
  - Peuvent traverser mais pas commencer depuis un système déja activé
  - Ne peuvent pas traverser un systeme si vaisseaux ennemis
  - Peuvent récuperer infanterie et chassseurs de systèmes qu'ils traversent s'ils ne contiennent pas de pion de commandement
- Canon spatial joueurs sur vaisseaux adverses
- Combat
  - **1** Barrage anti chasseur joueurs sur chasseurs adverses
  - **2** Annonce de retraite joueurs en commencant par le défenseur (si système adjacent valide)
    - placer infanterie dans ses vaisseaux
  - **2** dommages
  - **4** retraite
  - **5** recommencer à **2** tant qu'il reste des vaisseaux adverses dans le système
- Rééquilibrage capacité des vaisseaux (tuer de l'infanterie ou chasseurs si besoin)
- Invasion
  - Bombardement sur infanterie (sauf si système de défense planétaire)
  - invasion infanterie (choix de combien d'infanterie sur quelle planete)
  - Canon spatial défenseur sur infanterie de l'invasion
  - Combat jusqu'a extermination des infanteries advereses
  - Destruction des structures adverses si victoire attaquant
- Production si dock spatiaux dans le système

## 3. Phase de statut

- Chaque joueur peut scorer des VP, Max 1 objectif public + 1 secret
- Révéler 1 objectif public
- Dans l’ordre d’initiative chaque joueur pioche 1 carte action
- Retirer pions de commandement utilisés
- Gagner 2 pions de commandement
- Redistribuer ses pions de commandement
- Restaurer ses cartes
- Réparer ses unités rendre ses cartes stratégiques

### Distribution des pions de commandement

- **Tactique**: Utilisés pour activer des systèmes lors de la phase d'action: démarre une action tactique
- **Flotte**: Nombre maximal de vaisseaux sauf chasseur dans ses systèmes
- **Stratégique**: Pour utiliser l'action secondaire des cartes stratégies (+ certaines actions raciales)

## 4. Phase de projet

- Projet 1
- Projet 2
- Restaurer les planètes

### Fonctionnement

- Les votes commencent par la personne à gauche de l’orateur , sens horaire
- Possible de passer un vote

---

## Echanges commerciaux

- Pendant son tour chaque joueur peut effectuer jusqu'à un échange avec chacun de ses voisins (qui possede des vaisseaux dans des systèmes adjacent aux siens)
- A n'importe quel moment, même en combat
- N'importe quel joueur si **phase de projet**

### Peut échanger

- Des commodités pour les transformer en biens commerciaux
- Des biens commerciaux

## Technologies

### Conditions pour jouer une carte tech

`Tech card requirements (bottom left) <= Requirement with tech cards (bottom right) + Epuise planets tech`

### Types de tech

- **Bleu Propulsion**: Meilleurs déplacement pour ses vaisseaux
- **Rouge Militaire**: Améliorer la force de ses unités en combat
- **Jaune Cybernétique**: Amélioration unités terrestres/constructions (production, déplacement, défense planete)
- **Vert Biotique**: plus de ressources strat et cartes action + combat tricks

## Orateur

- Premier à drafter carte stratégie
- Vote en dernier pendant la phase projet, tranche les égalités

## Extra

- Sur une carte planête, valeur de ressource en jaune, influence en bleu
- Pas possible de scorer d'objectif public si ne contrôle pas toutes les planêtes de son système natal
- 7 cartes action max hand size
- Trois objectifs secrets max pendant tout le jeu (validé+non validé), si pioche -> doit défausser 1 objectif non validé puis shuffle deck objectif
- Max 2 SDP et 1 dock par planête
- Une planête vide déjà visitée appartient au dernier joueur qui l'a visité (placer un jeton dessus quand quitte la planête)
- Production d'unités limité par les unités restantes dans sa reserve sauf chasseurs et infanterie. Si out dans la réserve : le joueur peut detruire des unités qu'il souhaite produire, de ses systèmes non activés pour les remettre dans sa reserve au début de la phase de production.
