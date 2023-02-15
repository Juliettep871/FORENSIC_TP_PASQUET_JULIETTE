## Préparation ##

Pour réaliser l’analyse, l’environnement utilisé est une machine virtuelle Kali Linux sur laquelle est installé le docker du site web qui a été attaqué. La connexion à la machine attaquée se fait en ssh via le port 2222. Les identifiants nous ont été fournis pour nous connecter

## Introduction

Le site Bosch-Cyber a été victime d'une attaque informatique, au cours de laquelle un attaquant a utilisé différents outils pour s'infiltrer dans le système. Notre objectif est de déterminer quelles informations l'attaquant a extraites du site et de proposer des recommandations pour éviter que cela ne se reproduise à l'avenir. Cette mission sera confiée à Juliette Pasquet.


## Méthodologie 

Pour effectuer cette analyse, plusieurs méthodes seront utilisées pour examiner le site et le serveur.
Les éléments suivants seront analysés :
 	• L'historique des commandes 
	• Les différents journaux (logs) 
	• L'analyse des données et des fichiers

## Résultats ##


La première étape consiste à accéder à l'historique des commandes. Pour ce faire, il faut saisir la commande suivante __« cat ~/.bash_history »__. Le résultat est le suivant :

![image](https://user-images.githubusercontent.com/125276800/219003713-992bf91f-8d5e-48c2-856b-160a5bbf2a67.png)

Les commandes exécutées ont été réalisées par le pirate informatique. Plusieurs points importants sont à noter. En effet, celui-ci a pu accéder au fichier contenant le mot de passe, à savoir : __/etc/passwd__ ainsi que le nom d’utilisation. La deuxième information important, est le __« ping 138.66.89.12 »__, qui représente sans doute la machine distance du hackeur. 
Une fois que l'attaquant a déterminé que la fonction ping fonctionnait correctement, il a accédé à la __"crontab"__ pour ajouter la ligne suivante :

![image](https://user-images.githubusercontent.com/125276800/219027806-9600f37f-80af-4e59-afe8-5b7f66c62f35.png) 

La ligne de crontab, permet d’ouvrir une connexion TCP vers adresse IP __« 138.6689.12 »__ sur le port 444 et redirige les entrées/sorties de __bin/bash__ vers cette connexion. Elle permet donc de créer une connexion shell à distance pour exécuter des commandes

En conclusion, en revenant à l'historique des commandes, on peut remarquer la création d'un fichier zip nommé __"b0sch_cyber_tools"__, qui doit correspondre aux outils introduits par le hacker. Celui-ci l'a déplacé dans le répertoire __"/opt/leak"__. De plus, l'attaquant a supprimé le fichier __"/tmp/mypasswd"__, qui contenait le mot de passe permettant d'ouvrir le fichier zip. Il est important de déterminer quelles informations ont été extraites du système.

La deuxième étape consiste à analyser les différents logs afin de déterminer l'heure et la date de l'attaque. Ces fichiers se trouvent dans le répertoire __"/var/log"__. D'après nos suppositions, l'attaque a commencé le 3 septembre dans la matinée. Les logs permettent également de constater que plusieurs connexions ont échoué le 3 septembre entre 12h et 13h. Vous trouverez ci-dessous le fichier __"error.log"__:

![image](https://user-images.githubusercontent.com/125276800/218995982-150799f8-de7f-4090-8629-d80dd8029c87.png)

Le premier message d'erreur signalé dans le fichier "error.log" indique que la poignée de main SSL a échoué en raison d'une clé partagée incorrecte entre le client et le serveur. Cette erreur peut être liée à un problème avec le certificat SSL installé ou avec la configuration du serveur web. Toutefois, cette information ne nous permet pas de tirer de conclusion concluante.

Le deuxième message d'erreur indique que le serveur web a reçu une demande HTTP GET pour le fichier __"info.php"__, mais que le script PHP n'a pas réussi à récupérer les valeurs des en-têtes http.
De plus, il est nécessaire d’accéder au fichier __« access.log »__ :

![image](https://user-images.githubusercontent.com/125276800/219000474-35f24fb6-aa78-4f20-b7fc-06f1d1473b31.png).

Il est nécessaire de filtrer les lignes afin de trouver un résultat concluant. Pour cela, on utilise la commande __« grep ‘138.66.89.12’ | grep ‘pass’ access.log »__. Cette commande permet de filtrer les lignes de log pour trouver celles qui contiennent à la fois l'adresse IP de l'attaquant (138.66.89.12) et le mot-clé "pass", ce qui permet de trouver des informations sur le mot de passe du zip.







Cela ressort le résultat suivant :


![image](https://user-images.githubusercontent.com/125276800/219055206-4be0d988-cdad-4b22-afb7-f45b2cb156c7.png)

Nous avons maintenant le mot de passe, nous pouvons donc exploiter le fichier zip trouvé précédemment. Pour rappel, le fichier se situe dans __/opt/leak__

![image](https://user-images.githubusercontent.com/125276800/219037555-f0a1fa09-3e0a-41bf-8a43-4c4e27b97f94.png).

Avant de le dézipper, il est nécessaire de vérifier le contenu du fichier avec la commande :__«  unzip -d »__.

![image](https://user-images.githubusercontent.com/125276800/219041152-af82332f-b4e2-4827-afcc-fa0c98a2b993.png)

Il contient donc un fichier __.txt__

Nous pouvons maintenant décompresser le fichier, cependant nous obtenons une erreur de droits. Pour y remédier, nous devons exécuter la commande suivante : __"unzip bosh_cyber_tools.zip -d /home/b0osh/"__. Cela va déplacer le fichier dans le répertoire __"/home/b0osh"__. Il est nécessaire de taper le mot de passe que nous avons trouvé dans les logs pour décompresser le fichier.

Nous pouvons maintenant ouvrir le fichier, on retrouve :

![image](https://user-images.githubusercontent.com/125276800/219041043-ff8562ff-b08f-4ae9-991d-408ed6bed96c.png)

Une des étapes qui a été réalisé à été d’ouvrir le fichier de configuration du site web qui est situé dans le répertoire : __ « /var/www/html »__. Le seul fichier présent est __« index.html »__, la configuration est la suivante :

![image](https://user-images.githubusercontent.com/125276800/219065105-9d269800-2fbf-40dd-9f82-309567813e06.png)

Cela indique que le fichier est en maintenance.


## Conclusion ##

Au vu de l'analyse effectuée, nous pouvons en déduire qu’une faille est présente ce qui a permis à l’attaquant de permettre dans le système. Cette faille est surement liée aux différentes requêtes que nous avons trouvées dans les logs. De plus, la présence de la tache planifiée dans la crontab laisse la possibilité à l’attaquant de se connecter sur la machine.



## Recommandations ##

Pour corriger la faille, la première chose à faire est de sécuriser le serveur web en HTTPS, cela va permettre par exemple de régler les problèmes présents dans les log. Il sera aussi nécessaire de supprimer la tache planifiée présent dans la crontab et bloquer le port 4444 qui était utiliser par le hacker. Celui-ci ayant réussi à accéder au système, il est indispensable de changer le mot de passe en suivant les recommandations de l’ANSSI.
## Conclusion générale ##

Le site Bosch Cyber a subi une attaque informatique via une faille de sécurité. L’attaquant à prendre la main sur le serveur à distance, nous avons déterminé cela grâce aux logs, l’historique de commande et à la crontab.

Pour conclure, voici les commandes utilisées :

* ~/.bash_history : voir l’histoirique des commandes
* grep ‘138.66.89.12’ | grep ‘pass’ access.log : trouver le mot de passe pour le fichier zip
* unzip bosh_cyber_tools.zip -d /home/b0osh : dézipper le fichier et le déplacer
