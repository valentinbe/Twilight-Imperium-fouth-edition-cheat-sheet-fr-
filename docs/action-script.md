# Script de l'action tactique

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
