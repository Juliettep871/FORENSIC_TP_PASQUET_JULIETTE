## Exercice :
Le but est de lister les lignes dont :
-    La commune est «Carantec»
-    indice_de_repetition ne doit pas être vide
-    L’année est égale à 2019 

<corps>
	<details>
		<summary> Correction: </summary>

awk -F ';' '$5!="" && $1=="2019" && $9 ~ /Carantec/ {print}' consommation-annuelle-residentielle-par-adresse.csv

Nous utilisons l'option -F pour spécifier que le délimiteur de champ est le point-virgule, et nous ajoutons trois conditions : $5!="" pour exiger que la cinquième colonne ne soit pas vide, $1=="2019" pour exiger que la première colonne soit égale à 2019 et $9 ~ /Canteleu/ pour exiger que la neuvième colonne contienne la chaîne de caractères " Carantec ". Si ces trois conditions sont satisfaites, la commande AWK imprime la ligne entière.

<corps>
	<details>
		<summary> Résultat : </summary>

![image](https://user-images.githubusercontent.com/125276800/218736490-a2012d67-71d7-4549-aa92-bfd309809f1d.png)

.
