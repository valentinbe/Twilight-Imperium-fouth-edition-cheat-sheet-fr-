# Twilight Imperium fouth edition cheat sheet (fr)

## Distribution des pions de commandement

- **Tactique**: Utilisés pour activer des systèmes lors de la phase d'action: démarre une action tactique
- **Flotte**: Nombre maximal de vaisseaux sauf chasseur dans ses systèmes
- **Stratégique**: Pour utiliser l'action secondaire des cartes stratégies (+ certaines actions raciales)

## Phase de stratégie

Draft une carte, sens horaire en commencant par l'orateur, puis mettre 1 bien commercial sur les cartes restantes

### Explication des cartes

- **1 Gouvernance\***: + d'actions (+3 pions commandement)
- **2 Diplomatie**: Se protéger des attaques ennemies + restaurer ressources du système
- **3 Politique\***: Piocher cartes actions + pouvoirs dans décisions politiques (choix orateur + scry 2 paquet projet)
- **4 Construction\***: Construire structures sur planètes (pour défense et prod d'unités) **2 SDP max par planête**
- **5 Commerce\***: Faire du commerce et gagner des ressources (Biens commerciaux + commodités)
- **6 Guerre\***: Se déplacer deux fois avec un vaisseau ou se déplacer et produire sur un même système en un round + modifier sa stratégie en cours de round
- **7 Technologie\***: Recherche de technologies
- **8 Intrigue**: Scorer des points de victiore + piocher objectif secret

_Bold si bon choix en début de partie_

## Phase d'action

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

## Echanges commerciaux

Echanger des commodités pour les transformer en biens commerciaux:

- Joueur actif avec joueurs qui possedent des vaisseaux dans des systèmes adjacent aux siens
- N'importe quel joueur si **phase de projet**

## Technologies

### Conditions pour jouer une carte tech

`Tech card requirements (bottom left) <= Requirement with tech cards (bottom right) + Epuise planets tech`

### Types de tech

- **Bleu Propulsion**: Meilleurs déplacement pour ses vaisseaux
- **Rouge Militaire**: Améliorer la force de ses unités en combat
- **Jaune Cybernétique**: Amélioration unités terrestres/constructions (production, déplacement, défense planete)
- **Vert Biotique**: plus de ressources strat et cartes action + combat tricks

[List des cartes technologiques](https://docs.google.com/document/d/1RH-J1qfwcSp1MSnlVbuovZd_OCDqhA4XgvIC2ayS0pw/edit?usp=sharing)

## Extra

- Pas possible de scorer d'objectif public si ne contrôle pas toutes les planêtes de son système natal
- 7 cartes action max hand size
- Possible de jouer des cartes action après avoir passé
- Trois objectifs secrets max pendant tout le jeu (validé+non validé), si pioche -> doit défausser 1 objectif non validé puis shuffle deck objectif
- Max 2 SDP et 1 dock par planête

## Script de l'action tactique

```ts
// déplacement
attaquant.deplacement();
if (attaquant.canon_spatial.length || defenseur.canon_spatial.length) {
  joueurs.canon_spatial.roll(vaisseaux);
}
// combat
if (defenseur.vaisseaux.length) {
  // barrage anti chasseur
  if (
    attaquant.barrage_anti_chasseur.length ||
    defenseur.barrage_anti_chasseur.length
  ) {
    joueurs.barrage_anti_chasseur.roll(vaisseaux.chasseurs);
  }
  while (
    !joueurs.some(joueur => joueur.retraite) &&
    attaquant.vaisseaux.length &&
    defenseur.vaisseaux.length
  ) {
    // annonce de retraite
    [defenseur, attaquant].forEach(joueur => {
      joueur.systemes_de_retraite = systemes_adjacent.filter(
        systeme =>
          !systeme.vaisseaux.some(
            vaisseau => vaisseau.proprietaire_id !== joueur.id
          ) &&
          (systeme.unités.some(unité => unité.proprietaire_id === joueur.id) ||
            systeme.planetes.some(
              planete => planete.proprietaire_id === joueur.id
            ))
      );
      if (joueur.systemes_de_retraite.length) {
        joueur.retraite = joueur.choix();
        if (joueur.retraite) {
          joueur.vaisseaux.map(vaisseau => {
            vaisseau.infanteries.push(joueur.choix(joueur.infanteries));
          });
        }
      }
    });
    // lancer de combat et attribution des touches
    joueurs.vaisseaux.roll(vaisseaux);
    // retraite
    if (joueurs.some(joueur => joueur.retraite)) {
      joueurs
        .filter(joueur => joueur.retraite)
        .forEach(joueur => {
          const systeme_de_retraite = joueur.choix(joueur.systemes_de_retraite);
          joueur.vaisseaux.move(systeme_de_retraite);
          if (!systeme_de_retraite.pion_de_commandement) {
            systeme_de_retraite.pion_de_commandement =
              joueur.reserve.pion_de_commandement ||
              renforts.pion_de_commandement;
          }
        });
    }
  }
  // rééquilibrage des capacités
  joueurs.forEach(joueur => {
    const unités_capacité = joueur.vaisseaux.flatMap(vaisseau => [
      ...vaisseau.infanteries,
      ...vaisseau.chasseurs
    ]);
    while (
      unités_capacité.length >
      joueur.vaisseaux.reduce((el, sum) => sum + el.capacité)
    ) {
      joueur.destroy(joueur.choix(unités_capacité));
    }
  });
}
// invasion
if (!defenseur.vaisseaux.length) {
  if (attaquant.vaisseaux.some(vaisseau => vaisseau.bombardement)) {
    const vaisseaux_avec_bombardement = attaquant.vaisseaux.filter(
      vaisseau => vaisseau.bombardement
    );
    vaisseaux_avec_bombardement.forEach(vaisseau => {
      const planete_bombardée = attaquant.choix(
        planetes.filter(planete => !planete.bouclier_planetaire)
      );
      bombardement.roll(planete_bombardée.infanterie);
    });
  }
  planetes.forEach(planete => {
    const infanterie_invasion = attaquant.choix(attaquant.vaisseaux.infanterie);
    if (defenseur.canon_spatial.length) {
      canon_spatial.roll(infanterie_invasion);
    }
    while (infanterie_invasion.length || defenseur.infanterie.length) {
      infanterie.roll(infanterie);
    }
    if (infanterie_invasion.length) {
      defenseur.structures.destroy();
    }
  });
}
// production
if (attaquant.dock_spatiaux.length) {
  attaquant.production();
}
```