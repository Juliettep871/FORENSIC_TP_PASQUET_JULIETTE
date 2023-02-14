## Exercice :
Le but est de lister les lignes dont�:
-��� La commune est �Carantec�
-��� indice_de_repetition ne doit pas �tre vide
-��� L�ann�e est �gale � 2019�

<corps>
	<details>
		<summary> Correction: </summary>

awk -F ';' '$5!="" && $1=="2019" && $9 ~ /Carantec/ {print}' consommation-annuelle-residentielle-par-adresse.csv

Nous utilisons l'option -F pour sp�cifier que le d�limiteur de champ est le point-virgule, et nous ajoutons trois conditions : $5!="" pour exiger que la cinqui�me colonne ne soit pas vide, $1=="2019" pour exiger que la premi�re colonne soit �gale � 2019 et $9 ~ /Canteleu/ pour exiger que la neuvi�me colonne contienne la cha�ne de caract�res " Carantec ". Si ces trois conditions sont satisfaites, la commande AWK imprime la ligne enti�re.

<corps>
	<details>
		<summary> R�sultat�: </summary>

![image](https://user-images.githubusercontent.com/125276800/218736490-a2012d67-71d7-4549-aa92-bfd309809f1d.png)

.
