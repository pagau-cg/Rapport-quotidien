# Prototypes terrain — CG x Somtech

Prototypes de la couche terrain de la plateforme maison que Construction Gauthier
développe avec Somtech. Objectif : boucler **planifié vs réel** par la valeur acquise,
saisie au lot WBS, directement sur le chantier.

> Ces prototypes sont des **références de comportement**, pas du code de production.
> Ils serviront à Somtech pour rebâtir les modules dans la stack cible (Next.js / Supabase).
> Les données sont fictives, en mémoire.

## Pour l'équipe de direction — comment commenter

1. Ouvre chaque prototype (fichiers ci-dessous) dans un navigateur, idéalement sur **téléphone** : ils sont pensés mobile d'abord.
2. Essaie la saisie : change une quantité, coche un travailleur, bascule en vue semaine.
3. Laisse tes commentaires dans l'onglet **Issues** de ce dépôt, ou en commentaire direct si tu préfères.

## Les deux prototypes

| Module | Fichier | Qui l'utilise |
|---|---|---|
| Rapport quotidien de chantier | [`rapport-quotidien-cg-prototype.html`](rapport-quotidien-cg-prototype.html) | Contremaître |
| Feuille de temps forces propres | [`feuille-temps-forces-propres-cg-prototype.html`](feuille-temps-forces-propres-cg-prototype.html) | Travailleur |

### Rapport quotidien
Saisie de l'avancement par lot WBS en **quantité**; l'avancement, la valeur acquise et la
productivité (IPC) se calculent en direct. Lots tirés du WBS Somtech, contrats
sous-traitants de Maestro, effectif de l'affectation Somtech. Soumission signée et verrouillable.

### Feuille de temps forces propres
Saisie **quotidienne** par activité (projet → activité WBS en cascade → heures à la main),
avec une **vue hebdomadaire** récapitulative avant de soumettre la semaine. Les heures sont
validées par le contremaître avant la paye (Maestro).

## Principe directeur

La saisie terrain ne recrée presque rien : elle **tire de trois sources** et ne capte que
l'avancement physique, la présence et les heures.

- **Lots** → WBS du projet (Somtech), comptes de contrôle des zones actives
- **Contrats de sous-traitance** → Maestro
- **Effectif** → affectation Somtech + Ma Place RH
- **Photos / fichiers** → ACC
- **Vérité financière (paye, coûts réels)** → Maestro

## Contexte complet

Voir [`contexte-rapport-quotidien-valeur-acquise-CG-Somtech.md`](contexte-rapport-quotidien-valeur-acquise-CG-Somtech.md)
pour l'architecture, le modèle de données, les décisions verrouillées et la question
restante à trancher.
