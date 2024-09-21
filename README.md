# Credit_Risk_Modeling

Ce projet consiste à modéliser le risque de crédit en utilisant des données du Lending Club.

## Credit Risk Modeling - Preparation

Ce fichier a pour but de construire le modèle sur lequel nous allons estimer la probabilité de défaut (PD).

1 - Importation des données : Nous avons d'abord importé les données brutes provenant de Lending Club, puis créé une copie de sauvegarde pour éviter toute perte ou modification accidentelle des données originales lors du traitement.

2 - Exploration des données : Nous explorons ensuite les données pour connaître toutes les colonnes et leur valeurs.

3 - Nettoyage et transformation des variables continues : Nous avons commencé par nettoyer et transformer certaines variables continues afin qu'elles soient prêtes pour le modèle de régression.

4 - Traitement des valeurs manquantes et des formats de données : Nous avons traité les valeurs manquantes et converti les formats de données inadéquats.

5 - Définition de la variable cible : Nous avons défini la valeur cible (Good ou Bad) à partir de la colonne du statut du prêt. Nous affectons la valeur de 1 pour "Bad" quand il y a eu un retard lorsqu'une échéance du prêt n'a pas été remboursée sous 31 jours ou plus, et 0 quand le prêt est sain.

6 - Séparation des variables explicatives et de la cible : Nous avons séparé les variables explicatives et la variable cible pour la modélisation en créant 4 fichiers.

7 - Création des variables fictives ("dummy variables") : Notre but est de créer un modèle avec seulement des variables fictives (ou "dummy variables"), pour chaque catégorie définie, ce qui permet de transformer les variables continues ou discrètes en variables catégorielles binaires.

8 - Évaluation des variables avec WoE ("Weight of Evidence") : Nous avons évalué les différentes variables indépendantes en fonction de leur WoE, ou "Weight of Evidence", et utilisé les librairies matplotlib.pyplot et seaborn pour visualiser des représentations graphiques de nos variables.

9 - Sélection des catégories de référence : Lors de la création des différentes "dummy variables" pour l'entraînement du modèle, nous avons choisi d'enlever une "dummy variable" par catégorie, qui servira de base de comparaison.

10 - Application des transformations sur les données de test : Lorsque nous avons terminé avec le travail sur les données Train, nous avons effectué les mêmes opérations sur les données Test.

11 - Validation et vérification de la cohérence des données : Après s'être assuré que nos données Train et Test ont le même nombre de colonnes, nous avons sélectionné les "dummy variables" que nous avons créées, enlevé les catégories de références et appliqué une régression logistique.

12 - Évaluation de la performance du modèle : Après la mise en place du modèle de régression logistique, nous avons vérifié la performance du modèle sur les données de validation et ajusté les hyperparamètres si nécessaire. Nous avons utilisé des métriques de performance comme l'AUC pour évaluer la capacité prédictive du modèle.

13 - Interprétation des résultats : Enfin, nous avons interprété les résultats pour identifier les variables ayant le plus d'impact sur la probabilité de défaut.

## Credit Risk Modeling - PD Model Estimation

Ce fichier se concentre sur l'estimation de la Probabilité de Défaut (PD) à l'aide d'un modèle de régression logistique. Il inclut l'importation des données, l'entraînement du modèle sur un ensemble d'entraînement, et la prédiction des probabilités de défaut sur un ensemble de test. Les performances du modèle sont ensuite évaluées à l'aide de métriques comme l'AUC pour mesurer sa précision.

1 - Importation des bibliothèques : Le fichier commence par l'importation des bibliothèques nécessaires, telles que pandas, numpy, et des modules de sklearn pour la modélisation, ainsi que des outils spécifiques pour la création de scorecards comme woe et scorecardpy.

2 - Chargement des données d'entraînement et de test : Les données loan_data_inputs_train, loan_data_targets_train, loan_data_inputs_test, et loan_data_targets_test sont importées. Elles contiennent les variables explicatives (features) ainsi que la variable cible (default or not default).

3 - Création des WoE (Weight of Evidence) : Les variables catégorielles et continues sont transformées en WoE pour simplifier les relations entre les variables explicatives et la variable cible (défaut). Cette transformation permet de mieux capturer les risques associés à chaque classe des variables.

4 - Calcul des IV (Information Value) : L'Information Value (IV) est calculée pour chaque variable. Ce processus permet d'identifier la force prédictive des variables explicatives dans la modélisation du risque de crédit. Les variables avec une IV élevée sont considérées comme ayant un fort pouvoir prédictif.

5 - Entraînement du modèle de régression logistique : Un modèle de régression logistique est construit en utilisant les variables WoE pour estimer la Probabilité de Défaut (PD). Cette approche aide à minimiser les problèmes de colinéarité entre les variables.

6 - Création de la scorecard : Une scorecard est générée à partir du modèle de régression logistique. Chaque variable WoE est transformée en un score qui peut être interprété en termes de risque. Les points attribués à chaque emprunteur sont basés sur leur profil, et ces scores sont additionnés pour créer le score global de crédit.

7 - Conversion des probabilités en scores de crédit : Le modèle de régression logistique produit des probabilités de défaut, mais celles-ci sont converties en un "credit score" basé sur une échelle prédéfinie, comme la méthode de scaling du scorecard (e.g., score minimum et score maximum sur une échelle de 300 à 850).

8 - Prédiction des scores de crédit : Après l'entraînement du modèle, les scores de crédit sont prédits pour les données de test. Ces scores sont utilisés pour évaluer la capacité d'un emprunteur à rembourser un prêt ou pour prédire la probabilité de défaut.

9 - Évaluation avec l'AUC : L'AUC (Area Under the Curve) est calculée pour évaluer la performance du modèle en termes de séparation entre les emprunteurs à haut risque et à faible risque. Un AUC élevé indique une bonne capacité du modèle à différencier ces deux groupes.

10 - Interprétation et ajustement de la scorecard : Les résultats sont interprétés pour s'assurer que les scores produits correspondent aux attentes en termes de gestion des risques. Des ajustements peuvent être effectués sur les variables utilisées ou la méthodologie si nécessaire pour améliorer la précision des scores.

## Credit Risk Modeling - Monitoring

Ce fichier met en place un système de surveillance continue pour évaluer la performance du modèle PD sur de nouvelles données. Il surveille les écarts entre les prédictions actuelles et les données historiques, détecte les dérives dans les variables et ajuste le modèle si nécessaire. Un ré-entraînement périodique est prévu pour maintenir la précision du modèle au fil du temps.

1 - Importation des bibliothèques : Importation des bibliothèques essentielles comme pandas, numpy et des outils de visualisation pour évaluer les résultats au fil du temps.

2 - Chargement des données prétraitées : Les données précédemment nettoyées et divisées en jeux d'entraînement et de test sont rechargées pour effectuer la surveillance continue du modèle.

3 - Création d'un framework de surveillance : Mise en place d'un système de monitoring des performances du modèle sur les nouvelles données qui seront collectées au fil du temps.

4 - Calcul des scores PD sur les nouvelles données : Application du modèle PD déjà entraîné sur les nouvelles données pour évaluer la probabilité de défaut.

5 - Mise en place de métriques de performance : Utilisation de l'AUC et d'autres métriques comme le taux de faux positifs pour détecter les déviations de performance.

6 - Analyse des tendances historiques : Comparaison des prédictions récentes avec les performances historiques pour identifier les écarts significatifs.

7 - Détection des dérives de concept : Surveillance des changements dans la distribution des variables explicatives et des cibles pour s'assurer que le modèle reste valide dans des conditions changeantes.

8 - Ajustements du modèle : Lorsque des écarts significatifs sont détectés dans les prédictions, ajustement du modèle ou mise à jour avec de nouvelles données.

9 - Ré-entraînement périodique du modèle : Mise à jour du modèle en ré-entraînant avec des données plus récentes pour s'assurer que les prévisions restent fiables.

10 - Rapports réguliers : Génération de rapports périodiques montrant la performance du modèle, les ajustements effectués et les tendances observées dans les résultats.


## Credit Risk Modeling - Calculating Expected Loss Complete

Ce fichier se concentre sur le calcul de la Perte Attendue (Expected Loss) en combinant la Probabilité de Défaut (PD), l'Exposition au Défaut (EAD) et la Perte en cas de Défaut (LGD). Il calcule les pertes potentielles pour chaque emprunteur et agrège ces pertes pour obtenir une estimation totale des pertes attendues. Le fichier permet d'interpréter les résultats pour une meilleure gestion des risques.

1 - Importation des bibliothèques et des données : Chargement des bibliothèques standards comme pandas et numpy ainsi que les jeux de données pour PD, EAD (Exposition au Défaut), et LGD (Loss Given Default).

2 - Préparation des variables PD, EAD et LGD : Nous avons chargé et nettoyé les données correspondant à ces trois composantes essentielles pour calculer la perte attendue.

3 - Exploration des données EAD et LGD : Nous avons exploré les distributions de l'exposition et des pertes associées pour identifier toute anomalie dans les données.

4 - Calcul de la Probabilité de Défaut (PD) : Le modèle PD entraîné précédemment est appliqué pour calculer les probabilités de défaut sur le portefeuille.

5 - Calcul de l'EAD : L'Exposition au Défaut est calculée pour chaque emprunteur en fonction de ses caractéristiques financières, de la taille du prêt et d'autres facteurs.

6 - Calcul du LGD : La perte en cas de défaut est estimée en fonction des recouvrements possibles sur les prêts défaillants.

7 - Calcul de la Perte Attendue : Utilisation de la formule Expected Loss = PD * EAD * LGD pour estimer la perte attendue pour chaque prêt dans le portefeuille.

8 - Somme des pertes attendues : Agrégation de toutes les pertes attendues individuelles pour obtenir une estimation de la perte totale attendue sur le portefeuille.

9 - Visualisation des résultats : Utilisation de visualisations graphiques pour présenter les distributions des pertes attendues en fonction des différentes caractéristiques des emprunteurs.

10 - Interprétation des résultats : Analyse des résultats pour aider à la prise de décision en matière de gestion des risques, ajuster les politiques de prêt ou établir des réserves pour couvrir les pertes potentielles.
