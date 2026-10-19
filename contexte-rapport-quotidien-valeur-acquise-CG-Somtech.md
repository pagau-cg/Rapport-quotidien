# Contexte projet — Rapport quotidien, feuille de temps et valeur acquise (CG x Somtech)

Document de référence pour les sessions de travail sur les prototypes, et dossier à
transmettre à Somtech. Il encode l'architecture CG et les décisions déjà tranchées.
À mettre à jour quand une décision change.

## Objet

Concevoir, pour Construction Gauthier (CG), entrepreneur général québécois, les modules
terrain d'une plateforme maison développée avec Somtech : un **rapport quotidien de
chantier** (contremaître) et une **feuille de temps forces propres** (travailleur). Leur
fonction commune est de boucler planifié vs réel par la valeur acquise, saisie au lot WBS.

## Architecture CG (à respecter)

- Un seul objet projet, plusieurs vues. Le rapport quotidien et la feuille de temps sont
  des vues de saisie terrain de ce projet unique.
- Frontière des systèmes : un NOMBRE qui alimente une vue vit dans l'app (Somtech).
  Un FICHIER ou une PHOTO vit dans ACC (Autodesk Construction Cloud). La saisie terrain
  capte des nombres structurés; ses photos sont rattachées à ACC, pas stockées comme données.
- Propriété du champ : l'avancement (quantités réalisées par lot) appartient au chantier.
  Le contremaître ou le CP le saisit via le rapport. C'est sa source officielle.
- Maestro = vérité financière (coûts réels, heures réelles de paye). La saisie terrain ne
  remplace pas la paye Maestro; elle capte l'avancement physique, l'effectif et les heures.
- Ma Place RH = référentiel des employés (identités, rôles, actifs/inactifs).
- WBS (Annexe P-06) : le chantier avance par lots mesurables, chaque lot regroupant un ou
  plusieurs codes MasterFormat. L'avancement se saisit au lot.

## Provenance des listes (un champ = une source, tirer et lier)

La saisie terrain ne recrée presque rien : elle tire de trois sources et ne capte que
l'avancement physique, la présence et les heures.
- **Lots WBS** : la liste des lots vient du **WBS du projet défini dans Somtech** (module
  budget / activités budgétaires). Seuls les comptes de contrôle des **zones actives** sont
  présélectionnés dans le rapport; les autres restent activables au besoin, mais jamais la
  liste complète d'un coup.
- **Contrats de sous-traitance** : la liste des sous-traitants vient des **contrats octroyés
  dans Maestro**. Le contremaître coche ceux présents et indique l'effectif.
- **Effectif forces propres** : les travailleurs viennent de l'**affectation Somtech** (outil
  d'affectation nominale), avec identités et rôles de **Ma Place RH**. Le contremaître
  confirme présence et heures; il ne bâtit pas la liste.

## Cœur — avancement par lot WBS et valeur acquise

Chaque jour, le contremaître saisit, par lot WBS actif, la QUANTITÉ réalisée. Le %
se dérive (quantité cumulée sur quantité de référence du lot). La quantité est objective
et non manipulable; le % de jugement dérive vers l'optimisme.

GRANULARITÉ IMPOSÉE. Saisie au niveau COMPTE DE CONTRÔLE : 10 à 30 lots reportables par
projet maximum, regroupant les codes MasterFormat de la P-06 et calés sur les zones du
chantier (un lot = une zone x un lot de travaux mesurable). Jamais au niveau de granularité
de l'estimation. C'est ce qui rend la saisie quotidienne tenable.

TYPE DE LOT. Chaque lot est marqué autoexécuté (CG) ou sous-traité, parce que la valeur
acquise se calcule différemment :
- Autoexécuté : VA = quantité réalisée x valeur unitaire du lot; CR = heures CG validées x
  taux + matériel; la productivité (IPC) est le levier suivi.
- Sous-traité : VA = % avancement pour le décompte progressif du sous-traitant; CR = valeur
  contractuelle facturée, pas les heures CG; pas de productivité MOD.

LES TROIS MESURES ET LEUR SOURCE.
- VA (valeur acquise) = avancement saisi par le contremaître x budget du lot. Propriété du
  chantier.
- CR (coût réel) = heures validées (vers Maestro) + matériel et sous-traitants. Maestro = vérité.
- VP (valeur planifiée) = budget du lot étalé dans le temps selon la baseline d'échéancier
  (Last Planner / Visilean).

CE QUE L'APP CALCULE SEULE vs CE QUI EXIGE LA BASELINE.
- IPC (CPI) et productivité : calculables dès qu'on a VA et CR. Natif à l'app.
- IPD (SPI) et écart d'échéance : exigent une VP étalée dans le temps, donc une baseline
  d'échéancier. Si l'app ne la porte pas, l'IPD se ferme en joignant l'avancement à
  l'échéancier dans la couche BI (Power BI).

PAS DE MONTANTS POUR LE CONTREMAÎTRE. La valeur acquise se calcule en arrière-plan pour
pondérer l'avancement du projet, mais la vue contremaître n'affiche pas de dollars : seulement
l'avancement physique (quantité, %, reste) et la productivité (IPC). Les montants restent pour
le chargé de projet et le contrôle de projet.

## Modèle des heures (réconciliation, pas double saisie)

- Les travailleurs saisissent chacun leur propre feuille de temps (voir ci-dessous).
- Le contremaître saisit l'effectif et les heures du jour dans le rapport.
- Les heures du rapport servent de CONTRÔLE TERRAIN : les responsables réconcilient les
  feuilles de temps des travailleurs contre le rapport avant la paye, et les écarts sont signalés.
- Une fois validées, les heures approuvées partent vers Maestro et deviennent le CR.
- L'app ne recalcule pas la paye; elle capte l'effectif, l'avancement physique, et fournit le
  total de contrôle qui valide les feuilles de temps.

## Feuille de temps forces propres (travailleur)

- Saisie QUOTIDIENNE, par activité : le travailleur choisit le **projet** (liste déroulante),
  puis l'**activité de budget (WBS)** en cascade filtrée selon le projet choisi, puis entre
  ses **heures à la main** (pas de 0,25 h). Il peut découper sa journée sur plusieurs activités.
- VUE HEBDOMADAIRE avant soumission : un ruban de jours (Lun à Dim) montre le total de chaque
  jour; une vue semaine donne le récap par jour et par activité, et le total de la semaine.
  La soumission porte sur la SEMAINE complète, pas sur la journée.
- Les activités proposées viennent du WBS du projet (Somtech). Les heures soumises sont
  validées par le contremaître (réconciliation) avant la paye (Maestro).

## Exigences terrain

- Mobile d'abord : gros boutons tactiles, peu de saisie, lisible au soleil, une main.
- Rapide : rempli en quelques minutes.
- Tolérant hors-ligne : saisir puis synchroniser.
- Zéro jargon : pensé pour un contremaître ou un travailleur, pas un ingénieur.

## Le rapport quotidien — contenu

1. En-tête : projet, date, météo, quart.
2. Effectif et présences (affectation Somtech + Ma Place RH) : heures par personne; sert de
   contrôle de réconciliation des feuilles de temps.
3. Avancement par lot WBS (cœur) : quantité réalisée, % dérivé, cumul et reste.
4. Activités réalisées : court narratif.
5. Photos : rattachées à ACC (pas stockées comme données).
6. SST : points de sécurité, accueils, incidents, pauses.
7. Déficiences et non-conformités : consignées, avec suivi et notification.
8. Sous-traitants présents (contrats Maestro) : qui, combien, sur quel lot.
9. Livraisons reçues : matériaux et équipement (lien approvisionnement).
10. Équipement sur site (phase 2) : équipements Maestro affectés au chantier.
11. Soumission et signature : fin de journée, verrouillage, réouverture possible par le CP
    avec trace.

## Autres modules (parité Dreeven, branchés sur WBS et Maestro)

- Feuilles de temps : saisie par le travailleur, réconciliée par le rapport.
- Bons de travail et tâches.
- Approvisionnement : bons de commande, suivi commande, livraison, règlement.
- Gestion des déficiences.
- Dispatch et répartition : à unifier avec l'affectation nominale déjà spécifiée.
- Accès par rôle : calé sur la matrice d'autorisations CG.

## Identité visuelle

Aux couleurs de Construction Gauthier : vert de marque, charbon (noir du logo), blanc.
Entête vert CG avec rappel de marque, accents verts, lisibilité terrain (contraste élevé,
mode sombre). Pas d'autres couleurs d'accent que le vert CG; les couleurs sémantiques
(avertissement, erreur) restent réservées aux alertes.

## Livrables actuels

- Prototype fonctionnel du rapport quotidien (HTML autonome, mobile d'abord) : avancement
  par lot WBS en quantité, effectif, valeur acquise et IPC en direct, provenances système
  affichées, soumission signée et verrouillable.
- Prototype fonctionnel de la feuille de temps forces propres (HTML autonome) : saisie
  quotidienne en cascade projet → activité WBS → heures, vue hebdomadaire avant soumission.
- Ce document de contexte.

Les prototypes sont des RÉFÉRENCES DE COMPORTEMENT, pas du code de production; à rebâtir dans
la stack cible (Next.js / Supabase). Données de démonstration en mémoire, fictives.

## Décisions verrouillées (ne pas rouvrir)

1. Lots actifs : comptes de contrôle des zones en cours, présélectionnés. Activation ponctuelle
   d'un autre lot du WBS possible, jamais la liste complète.
2. Avancement : en QUANTITÉ. Le % se dérive.
3. Heures : le travailleur saisit sa feuille de temps (quotidienne, vue hebdo avant soumission);
   le rapport valide par réconciliation avant la paye; Maestro reste la vérité du CR.
4. Signature et verrouillage à la soumission : oui. Réouverture possible par le CP, avec trace.
5. Pas de montants ($) dans la vue contremaître; avancement physique et IPC seulement.
6. Provenances : lots du WBS Somtech, contrats ST de Maestro, effectif de l'affectation Somtech.

## Décision restante à trancher

L'app porte-t-elle la baseline d'échéancier étalée dans le temps, ou seulement le budget total
par lot ? Cela détermine si l'IPD (SPI) se ferme dans l'app ou dans Power BI. Lié : qui décide
qu'une zone est active (automatique selon l'échéancier / Last Planner, ou sélection manuelle du
CP en début de semaine).
