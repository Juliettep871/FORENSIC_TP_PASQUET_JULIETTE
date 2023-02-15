## Introduction
Le site Bosch-Cyber a subi une compromission, lors de laquelle un attaquant a utilisé différents outils pour s'infiltrer dans le système. Notre objectif est de déterminer quelles informations l'attaquant a extraites du site et de déterminer des recommandations afin que cela arrive plus. Cette mission sera effectuée par Pasquet Juliette.


## Méthodologie 

Pour effectuer cette analyse, plusieurs moyens seront utilisés pour analyser le site et le serveur.

Les points suivants vont être analysé :
* L’historique des commandes
* Les différents logs
* Analyse des données et fichiers

## Résultats ##


La première chose à faire est d’accéder à l’historique des différentes commandes. Pour cela il faut taper la commande __« cat ~/.bash_history »__. Le résultat est le suivant :

![image](https://user-images.githubusercontent.com/125276800/219003713-992bf91f-8d5e-48c2-856b-160a5bbf2a67.png)

Ses commandes ont été effectué par le hackeur, plusieurs points important sont à noter. En effet, celui-ci a pu accéder au fichier ou il est situé le mot de passe c’est-à-dire __/etc/passwd__ ainsi que le nom d’utilisation. La deuxième information important, est le __ « ping 138.66.89.12 »__, qui représente sans doute sa machine distance. 
Une fois que l’attaquant à déduit que le ping fonctionner bien, il à accéder à la __ « crontab »__ pour ajouter la ligne suivante :

![image](https://user-images.githubusercontent.com/125276800/219027806-9600f37f-80af-4e59-afe8-5b7f66c62f35.png) 

La ligne de crontab, permet d’ouvrir une connexion TCP vers adresse IP __« 138.6689.12 »__ sur le port 444 et redirige les entrées/sorties de __bin/bash__ vers cette connexion. Elle permet donc de créée une connexion shell à distance pour exécuter des commandes.

Pour finir, pour revenir à l’historique des commandes, on remarque la création d’un zip __« b0sch_cyber_tools »__, ce fichier doit correspondre  au outils introduit par le hacker. Celui-ci à déplacer le fichier dans le répertoire __/opt/leak__. De plus, il a supprimer le fichier __ « /tmp/mypasswd »__ dans lequel était renseigné le mot de passe pour ouvrir le zip.

La deuxième étape est consiste à analyser les différents log qui a pour but de déterminer l’heure et la date de l’attaque, les fichiers sont disponibles dans __/var/log__. Nous pouvons supposer que l’attaque à commencer le 3 septembre dans la matinée. Les logs permettent de déterminer que plusieurs connexions ont échoué le 3 septembre à partir de 12h-13h. Ci-dessous le fichier __ « error.log » __:

![image](https://user-images.githubusercontent.com/125276800/218995982-150799f8-de7f-4090-8629-d80dd8029c87.png)

Le premier message d'erreur indique que la poignée de main SSL a échoué en raison d'une mauvaise clé partagée entre le client et le serveur. Cela peut indiquer un problème avec le certificat SSL installé ou avec la configuration du serveur web. Mais cela n’apport rien de concluant 

Le deuxième message d'erreur indique que le serveur web a reçu une demande HTTP GET pour le fichier __"info.php"___, mais que le script PHP n'a pas réussi à récupérer les valeurs des en-têtes http.
De plus, il est nécessaire d’accéder au fichier __« access.log »__ :

![image](https://user-images.githubusercontent.com/125276800/219000474-35f24fb6-aa78-4f20-b7fc-06f1d1473b31.png).

Il est nécessaire de filtrer, les lignes afin de trouver un résultat concluant, pour cela on utilise la commande __ « grep ‘138.66.89.12’ | grep ‘pass’ access.log »__. Cette commande, permet de trouver le mot de passe dans les log.
Pour rappels, l’adresse IP est celle qu’on à trouver grâce à l’historique des commandes qui correspond donc à l’attaquant. 

Cela ressort le résultat suivant :




Nous avons maintenant le mot de passe, nous pouvons donc exploiter le zip trouver précédemment.
Pour rappel, le fichier se situe dans __/opt/leak__

![image](https://user-images.githubusercontent.com/125276800/219037555-f0a1fa09-3e0a-41bf-8a43-4c4e27b97f94.png).

Nous pouvons maintenant décompresser le fichier, cela nous ressors une erreur de droit. Il est nécessaire de le déplacer pour cela on exécuter la commmande suivante __«  unzip bosh_cyber_tools.zip -d /home/b0osh/ » »__ . On déplace le fichier, dans __/home/b0osh__, il est nécessaire de taper le mot de passe que nous avons trouvé dans les log.

Le fichier dézipper contient un fichier .txt :
