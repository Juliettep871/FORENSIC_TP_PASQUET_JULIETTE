## Pr�paration ##

Pour r�aliser l�analyse, l�environnement utilis� est une machine virtuelle Kali Linux sur laquelle est install� le docker du site web qui a �t� attaqu�. La connexion � la machine attaqu�e se fait en ssh via le port 2222. Les identifiants nous ont �t� fournis pour nous connecter

## Introduction

Le site Bosch-Cyber a �t� victime d'une attaque informatique, au cours de laquelle un attaquant a utilis� diff�rents outils pour s'infiltrer dans le syst�me. Notre objectif est de d�terminer quelles informations l'attaquant a extraites du site et de proposer des recommandations pour �viter que cela ne se reproduise � l'avenir. Cette mission sera confi�e � Juliette Pasquet.


## M�thodologie 

Pour effectuer cette analyse, plusieurs m�thodes seront utilis�es pour examiner le site et le serveur.
Les �l�ments suivants seront analys�s :
 	� L'historique des commandes 
	� Les diff�rents journaux (logs) 
	� L'analyse des donn�es et des fichiers

## R�sultats ##


La premi�re �tape consiste � acc�der � l'historique des commandes. Pour ce faire, il faut saisir la commande suivante __��cat ~/.bash_history��__. Le r�sultat est le suivant�:

![image](https://user-images.githubusercontent.com/125276800/219003713-992bf91f-8d5e-48c2-856b-160a5bbf2a67.png)

Les commandes ex�cut�es ont �t� r�alis�es par le pirate informatique. Plusieurs points importants sont � noter. En effet, celui-ci a pu acc�der au fichier contenant le mot de passe, � savoir : __/etc/passwd__ ainsi que le nom d�utilisation. La deuxi�me information important, est le __��ping 138.66.89.12��__, qui repr�sente sans doute la machine distance du hackeur. 
Une fois que l'attaquant a d�termin� que la fonction ping fonctionnait correctement, il a acc�d� � la __"crontab"__ pour ajouter la ligne suivante :

![image](https://user-images.githubusercontent.com/125276800/219027806-9600f37f-80af-4e59-afe8-5b7f66c62f35.png) 

La ligne de crontab, permet d�ouvrir une connexion TCP vers adresse IP __��138.6689.12��__ sur le port 444 et redirige les entr�es/sorties de __bin/bash__ vers cette connexion. Elle permet donc de cr�er une connexion shell � distance pour ex�cuter des commandes

En conclusion, en revenant � l'historique des commandes, on peut remarquer la cr�ation d'un fichier zip nomm� __"b0sch_cyber_tools"__, qui doit correspondre aux outils introduits par le hacker. Celui-ci l'a d�plac� dans le r�pertoire __"/opt/leak"__. De plus, l'attaquant a supprim� le fichier __"/tmp/mypasswd"__, qui contenait le mot de passe permettant d'ouvrir le fichier zip. Il est important de d�terminer quelles informations ont �t� extraites du syst�me.

La deuxi�me �tape consiste � analyser les diff�rents logs afin de d�terminer l'heure et la date de l'attaque. Ces fichiers se trouvent dans le r�pertoire __"/var/log"__. D'apr�s nos suppositions, l'attaque a commenc� le 3 septembre dans la matin�e. Les logs permettent �galement de constater que plusieurs connexions ont �chou� le 3 septembre entre 12h et 13h. Vous trouverez ci-dessous le fichier __"error.log"__:

![image](https://user-images.githubusercontent.com/125276800/218995982-150799f8-de7f-4090-8629-d80dd8029c87.png)

Le premier message d'erreur signal� dans le fichier "error.log" indique que la poign�e de main SSL a �chou� en raison d'une cl� partag�e incorrecte entre le client et le serveur. Cette erreur peut �tre li�e � un probl�me avec le certificat SSL install� ou avec la configuration du serveur web. Toutefois, cette information ne nous permet pas de tirer de conclusion concluante.

Le deuxi�me message d'erreur indique que le serveur web a re�u une demande HTTP GET pour le fichier __"info.php"__, mais que le script PHP n'a pas r�ussi � r�cup�rer les valeurs des en-t�tes http.
De plus, il est n�cessaire d�acc�der au fichier __��access.log��__�:

![image](https://user-images.githubusercontent.com/125276800/219000474-35f24fb6-aa78-4f20-b7fc-06f1d1473b31.png).

Il est n�cessaire de filtrer les lignes afin de trouver un r�sultat concluant. Pour cela, on utilise la commande __��grep �138.66.89.12� | grep �pass� access.log��__. Cette commande permet de filtrer les lignes de log pour trouver celles qui contiennent � la fois l'adresse IP de l'attaquant (138.66.89.12) et le mot-cl� "pass", ce qui permet de trouver des informations sur le mot de passe du zip.







Cela ressort le r�sultat suivant�:


![image](https://user-images.githubusercontent.com/125276800/219055206-4be0d988-cdad-4b22-afb7-f45b2cb156c7.png)

Nous avons maintenant le mot de passe, nous pouvons donc exploiter le fichier zip trouv� pr�c�demment. Pour rappel, le fichier se situe dans __/opt/leak__

![image](https://user-images.githubusercontent.com/125276800/219037555-f0a1fa09-3e0a-41bf-8a43-4c4e27b97f94.png).

Avant de le d�zipper, il est n�cessaire de v�rifier le contenu du fichier avec la commande :__�� unzip -d��__.

![image](https://user-images.githubusercontent.com/125276800/219041152-af82332f-b4e2-4827-afcc-fa0c98a2b993.png)

Il contient donc un fichier __.txt__

Nous pouvons maintenant d�compresser le fichier, cependant nous obtenons une erreur de droits. Pour y rem�dier, nous devons ex�cuter la commande suivante : __"unzip bosh_cyber_tools.zip -d /home/b0osh/"__. Cela va d�placer le fichier dans le r�pertoire __"/home/b0osh"__. Il est n�cessaire de taper le mot de passe que nous avons trouv� dans les logs pour d�compresser le fichier.

Nous pouvons maintenant ouvrir le fichier, on retrouve�:

![image](https://user-images.githubusercontent.com/125276800/219041043-ff8562ff-b08f-4ae9-991d-408ed6bed96c.png)

Une des �tapes qui a �t� r�alis� � �t� d�ouvrir le fichier de configuration du site web qui est situ� dans le r�pertoire�: __���/var/www/html��__. Le seul fichier pr�sent est __��index.html��__, la configuration est la suivante�:

![image](https://user-images.githubusercontent.com/125276800/219065105-9d269800-2fbf-40dd-9f82-309567813e06.png)

Cela indique que le fichier est en maintenance.


## Conclusion ##

Au vu de l'analyse effectu�e, nous pouvons en d�duire qu�une faille est pr�sente ce qui a permis � l�attaquant de permettre dans le syst�me. Cette faille est surement li�e aux diff�rentes requ�tes que nous avons trouv�es dans les logs. De plus, la pr�sence de la tache planifi�e dans la crontab laisse la possibilit� � l�attaquant de se connecter sur la machine.



## Recommandations ##

Pour corriger la faille, la premi�re chose � faire est de s�curiser le serveur web en HTTPS, cela va permettre par exemple de r�gler les probl�mes pr�sents dans les log. Il sera aussi n�cessaire de supprimer la tache planifi�e pr�sent dans la crontab et bloquer le port 4444 qui �tait utiliser par le hacker. Celui-ci ayant r�ussi � acc�der au syst�me, il est indispensable de changer le mot de passe en suivant les recommandations de l�ANSSI.
## Conclusion g�n�rale ##

Le site Bosch Cyber a subi une attaque informatique via une faille de s�curit�. L�attaquant � prendre la main sur le serveur � distance, nous avons d�termin� cela gr�ce aux logs, l�historique de commande et � la crontab.

Pour conclure, voici les commandes utilis�es�:

* ~/.bash_history�: voir l�histoirique des commandes
* grep �138.66.89.12� | grep �pass� access.log�: trouver le mot de passe pour le fichier zip
* unzip bosh_cyber_tools.zip -d /home/b0osh�: d�zipper le fichier et le d�placer
