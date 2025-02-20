## Classement aux Échecs

Lors de la comparaison des performances entre deux groupes, une erreur courante consiste à les comparer en fonction des meilleurs de chaque groupe, en particulier dans les scénarios dans lesquels l'un des groupes a un nombre d'échantillons significativement plus élevé que l'autre (surreprésenté).
Un tel exemple est aux échecs, où les gens ont essayé incorrectement de prétendre que les joueuses sont pires que les joueurs parce que si l'on considère les meilleurs joueurs, aucune femme n'a jamais été championne du monde, ou la différence d'Elo (une méthode pour calculer la compétence relative niveaux de joueurs) entre la meilleure femme et le meilleur homme est grande, etc.

Pourquoi est-ce une erreur? Puisque ça néglige le taux de participation de ces groupes.
Naïvement, si deux groupes sont tirés de la même distribution, le groupe surreprésenté aura plus de chances d'être dans les queues de la distribution.
Par exemple, disons qu'il y a deux groupes de personnes : A et B.
Le groupe A compte 10 personnes, le groupe B en compte 2.
Chacune des 12 personnes se voit attribuer au hasard un numéro entre 1 et 100 (avec remplacement).
Ensuite, prenez le maximum du groupe A comme score du groupe A et le maximum du groupe B comme score du groupe B.
En moyenne, le groupe A obtiendra un score d'environ 91 et le groupe B d'environ 67, où la seule différence entre les deux groupes est la taille.
Le plus grand groupe a plus de chances d'avoir un score élevé, donc obtiendra en moyenne un score plus élevé.
La manière équitable de comparer ces groupes de taille inégale consiste à comparer leurs moyennes (moyennes), et non leurs valeurs maximales.
Bien sûr, dans cet exemple, ce serait 50 pour les deux groupes – pas de différence !
Notez que cela ne fait que souligner l'effet statistique de la simple comparaison des groupes surreprésentés et sous-représentés et néglige tout problème potentiel ou autre biais dans les données.

Dans cette section, nous explorerons cela dans le contexte des classements d'échecs mentionnés précedemment.
Aucun code pour vous aider n'est fourni dans le fichier python ; à la place, vous pouvez suivre dans le cahier `chess_ratings.ipynb`.

### 1 Charger et nettoyer les données

- Complétez `chess_ratings.py:parse_xml()`
- Complétez `chess_ratings.py:clean_data()`

Un fichier XML (zippé) vous est fourni :

    ./data/standard_oct22frl_xml.zip

Il a été obtenu à partir de la base de données des joueurs FIDE qui peut être trouvée [ici] (http://ratings.fide.com/download.phtml?period=2022-10-01).
Notez que les notes sont mises à jour assez régulièrement, nous utilisons donc l'horodatage fixe du 06 octobre 2022, qui vous est fourni dans le répertoire de données.
Vous ne devriez pas avoir besoin de télécharger quoi que ce soit.

Complétez la méthode `parse_xml()` ; notez que la première partie de la méthode contient du code pour extraire automatiquement le fichier zip pour vous.
Assurez-vous d'obtenir les données suivantes à partir du XML (et de les utiliser comme noms de colonne) dans votre dataframe :

    ["nom", "classement", "sexe", "anniversaire", "pays", "drapeau", "titre"]

Ensuite, complétez `clean_data()`: supprimez les joueurs avec des anniversaires NaN, convertissez les types numériques en type approprié et filtrez les anniversaires `<=2002` (car les scores Elo pour les personnes de < 20 ans peuvent ne pas être fiables).

### 2 Comparer les distributions de notes

- Complétez `chess_ratings.py:bin_counts()`
- Complétez les graphiques dans `chess_ratings.ipynb`

Comparons maintenant la répartition des notes des joueurs masculins et féminins.
Étant donné que les données sont assez fines, nous devrons regrouper les notes.
Complétez les données de `bin_counts()`, qui devrait gérer le binning pour les données arbitraires et le choix des bacs.
En plus de renvoyer les décomptes bruts, renvoyez également les décomptes normalisés (`count_norm`).

Ensuite, vous utiliserez cette méthode pour compléter les graphiques dans `chess_ratings.ipynb`.
Pour les graphiques, choisissez une *largeur* de bac de 50, une plage de 1000 à 2900 (assurez-vous que votre plage s'étend jusqu'à 2900 !), et le *centre* des bacs comme point médian de chaque bac (c'est-à-dire pour un bac entre [ 1500, 1550), le centre serait 1525.).
Notez que lors de la définition de vos bacs, vous constaterez que `len(bin_centers) = len(bins) - 1`

À l'aide des données regroupées, tracez deux tracés linéaires des données regroupées côte à côte; l'un contenant les décomptes bruts (`"count"`), et l'autre contenant les décomptes normalisés (`"count_norm"`), et M/F devraient être de deux couleurs différentes.
Assurez-vous d'inclure ces graphiques dans votre cahier lorsque vous soumettez.

### 3 Tests de permutation

- Compléte `chess_ratings.py:PermutationTests.job()`
- Complétez `chess_ratings.py:sample_two_groups()`

Finalement, nous effectuerons les tests de permutation indiqués dans le thought experiment de l'introduction.
Prenez l'ensemble de données nettoyé complet (hommes et femmes) et échantillonnez au hasard deux groupes sans remplacement (c'est-à-dire mélangez les joueurs).
La taille des groupes doit refléter la différence du monde réel que nous souhaitons étudier, c'est-à-dire la taille du groupe masculin et féminin.
Terminez `PermutationTests.job()`, qui implémente la partie échantillonnage de cette  expérience, et renvoie la valeur maximale des groupes surreprésentés et sous-représentés respectivement.

Ensuite, complétez la méthode `sample_two_groups()`, qui exécute cette expérience `n_iter` de fois.
Une fois terminé, exécutez cette expérience dans le bloc-notes avec au moins `n_iter=1000`.
Exécutez la cellule qui imprime la différence moyenne obtenue à partir des tests de permutation, ainsi que les différences réelles.
Assurez-vous d'inclure ces résultats imprimés dans votre notebook lorsque vous les soumettez.

### 4 Questions

Répondez à ces questions (1 à 3 phrases chacune) dans la section spécifiée dans `chess_ratings.ipynb` :

1. Interprétez les résultats - pouvez-vous tirer une conclusion ? Rappelez-vous que l'affirmation discutée dans l'introduction de cette question était "les hommes sont meilleurs que les femmes aux échecs parce que la plupart des meilleurs joueurs sont des hommes". (*Remarque : probablement une partie de votre réponse ici sera liée à votre réponse à la question suivante.*)
2. Pensez-vous que les chiffres obtenus ici racontent toute l'histoire ? Quels pourraient être les problèmes avec l'analyse menée ici ? Les données avec lesquelles nous travaillons sont-elles biaisées d'une quelconque manière (autre qu'un biais de surreprésentation) ? L'ELO est-il une bonne mesure et peut-il être utilisé pour répondre à la question initiale ? Existe-t-il des différences dans le traitement social, culturel et systémique des hommes et des femmes qui peuvent empêcher le groupe sous-représenté d'obtenir des résultats similaires ? Rien d'autre?

Le but de ces questions est de souligner que les données sont une représentation limitée du monde réel.
Il est essentiel pour nous, en tant que scientifiques des données, de prendre du recul lorsque nous examinons un résultat et de réfléchir à la façon dont il est lié au monde réel, plutôt que de simplement supposer naïvement que les données et la configuration expérimentale sont bonnes, ce qui entraîne souvent des conclusions erronées/incorrectes.
Il pourrait y avoir plusieurs facteurs de causalité qui expliquent une relation qui sont indépendants de l'hypothèse d'origine: utilisation de données qui ne reflètent pas vraiment l'hypothèse que vous souhaitez tester, données biaisées (y compris les groupes surreprésentés), différences systémiques réelles entre les groupes, etc.


# Références

- **Chess Ratings** est basé sur l'analyse de cet [article de blog.](https://en.chessbase.com/post/what-gender-gap-in-chess)
