#Segmentation clients RFM d'un e-commerce

Projet réalisé pendant mon DU Data Analytics (Panthéon-Sorbonne) : segmenter les clients d'un détaillant en ligne avec la méthode RFM, puis traduire chaque segment en actions marketing et entraîner un modèle qui classe automatiquement les nouveaux clients dans un segment.

Objectif

Répondre à trois questions :

Quels groupes de clients se dégagent des données de transactions ?
Quelles actions marketing proposer à chacun ?
Peut-on affecter un nouveau client à un groupe à partir de ses seuls comportements d'achat ?
Données

Transactions d'un détaillant en ligne (jeu de données « Online Retail »). Source : [lien vers la source, à compléter].

Le fichier n'est pas inclus dans ce dépôt. Pour relancer le notebook, télécharge-le à la source puis charge-le dans Colab (cellule files.upload()).

Nettoyage appliqué (397 884 lignes de transactions conservées, 18 532 commandes distinctes) :

suppression des lignes sans identifiant client ;
suppression des quantités et prix unitaires négatifs ou nuls ;
suppression des factures annulées (numéros commençant par « C ») ;
correction des types (prix converti en nombre, dates converties en dates).
Méthode

1. Variables RFM, calculées par client

Recency : nombre de jours entre la dernière commande du client et une date de référence (un jour après la dernière transaction du jeu de données).
Frequency : nombre de commandes distinctes.
Monetary : montant total dépensé (quantité × prix unitaire).

2. Segmentation non supervisée

Standardisation des trois variables (StandardScaler).
Choix du nombre de groupes avec la méthode du coude, puis Bisecting K-Means avec 3 groupes.
Qualité de la segmentation : score de silhouette de 0,593.

3. Interprétation et recommandations

Segment	Profil	Actions proposées
Inactifs	Récence élevée, fréquence et montant faibles	Offres temporaires, sondage de satisfaction, identification des clients perdus (plus d'un an sans commande)
Réguliers	Récence modérée, fréquence et montant moyens	Programme de fidélité à points, ventes croisées et montées en gamme, offres personnalisées, newsletter ciblée
VIP	Récence faible, fréquence et montant élevés	Programme VIP (accès anticipé, ventes privées), gestionnaire de compte dédié, surveillance du désengagement

4. Classification des nouveaux clients

Modèles comparés : forêt aléatoire, K plus proches voisins, régression logistique.
Évaluation par découpage train/test et par validation croisée stratifiée à 5 plis (accuracy et F1 macro), avec rapports de classification et matrices de confusion.
Importance des variables RFM dans la forêt aléatoire.
Pour aller plus loin : VotingClassifier (vote pondéré par les probabilités) combinant les trois modèles.
Prédiction sur un nouveau client, avec le même standardiseur que lors de l'entraînement.
Analyses complémentaires

Visualisations par pays (quantités, dépenses, revenus, nombre de commandes) et analyse des prix par produit.

Limites
Les segments sont issus d'un algorithme non supervisé : leur nom (Inactifs, Réguliers, VIP) est une interprétation des profils, pas une vérité observée.
Le modèle de classification apprend à retrouver des segments construits à partir des mêmes trois variables : sa bonne performance confirme la cohérence des groupes, mais ne prouve pas leur valeur business. Il faudrait la tester sur des campagnes réelles.
Les recommandations marketing sont des pistes, pas des résultats mesurés.
Technologies

Python, Pandas, NumPy, Matplotlib, Scikit-learn, Google Colab

Utilisation
Ouvre le notebook dans Google Colab.
Charge le fichier de données quand la première cellule le demande.
Exécute les cellules dans l'ordre (Exécution > Tout exécuter).
Auteure

Noémie Anguille Charmille, DU Data Analytics (Panthéon-Sorbonne) et DU IA pour la Finance et l'Actuariat (Sorbonne Université). Contact : Lcharmille@gmail.com
