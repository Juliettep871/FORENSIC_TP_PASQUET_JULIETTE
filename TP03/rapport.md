## Introduction
Le site Bosch-Cyber a subi une compromission, lors de laquelle un attaquant a utilis� diff�rents outils pour s'infiltrer dans le syst�me. Notre objectif est de d�terminer quelles informations l'attaquant a extraites du site et de d�terminer des recommandations afin que cela arrive plus. Cette mission sera effectu�e par Pasquet Juliette.


## M�thodologie 

Pour effectuer cette analyse, plusieurs moyens seront utilis�s pour analyser le site et le serveur.

Les points suivants vont �tre analys�:
* L�historique des commandes
* Les diff�rents logs
* Analyse des donn�es et fichiers

## R�sultats ##


La premi�re chose � faire est d�acc�der � l�historique des diff�rentes commandes. Pour cela il faut taper la commande __��cat ~/.bash_history��__. Le r�sultat est le suivant�:

![image](https://user-images.githubusercontent.com/125276800/219003713-992bf91f-8d5e-48c2-856b-160a5bbf2a67.png)

Ses commandes ont �t� effectu� par le hackeur, plusieurs points important sont � noter. En effet, celui-ci a pu acc�der au fichier ou il est situ� le mot de passe c�est-�-dire __/etc/passwd__ ainsi que le nom d�utilisation. La deuxi�me information important, est le __���ping 138.66.89.12��__, qui repr�sente sans doute sa machine distance. 
Une fois que l�attaquant � d�duit que le ping fonctionner bien, il � acc�der � la __���crontab��__ pour ajouter la ligne suivante�:

![image](https://user-images.githubusercontent.com/125276800/219027806-9600f37f-80af-4e59-afe8-5b7f66c62f35.png) 

La ligne de crontab, permet d�ouvrir une connexion TCP vers adresse IP __��138.6689.12��__ sur le port 444 et redirige les entr�es/sorties de __bin/bash__ vers cette connexion. Elle permet donc de cr��e une connexion shell � distance pour ex�cuter des commandes.

Pour finir, pour revenir � l�historique des commandes, on remarque la cr�ation d�un zip __��b0sch_cyber_tools��__, ce fichier doit correspondre  au outils introduit par le hacker. Celui-ci � d�placer le fichier dans le r�pertoire __/opt/leak__. De plus, il a supprimer le fichier __���/tmp/mypasswd��__ dans lequel �tait renseign� le mot de passe pour ouvrir le zip.

La deuxi�me �tape est consiste � analyser les diff�rents log qui a pour but de d�terminer l�heure et la date de l�attaque, les fichiers sont disponibles dans __/var/log__. Nous pouvons supposer que l�attaque � commencer le 3 septembre dans la matin�e. Les logs permettent de d�terminer que plusieurs connexions ont �chou� le 3 septembre � partir de 12h-13h. Ci-dessous le fichier __ ��error.log���__:

![image](https://user-images.githubusercontent.com/125276800/218995982-150799f8-de7f-4090-8629-d80dd8029c87.png)

Le premier message d'erreur indique que la poign�e de main SSL a �chou� en raison d'une mauvaise cl� partag�e entre le client et le serveur. Cela peut indiquer un probl�me avec le certificat SSL install� ou avec la configuration du serveur web. Mais cela n�apport rien de concluant 

Le deuxi�me message d'erreur indique que le serveur web a re�u une demande HTTP GET pour le fichier __"info.php"___, mais que le script PHP n'a pas r�ussi � r�cup�rer les valeurs des en-t�tes http.
De plus, il est n�cessaire d�acc�der au fichier __��access.log��__�:

![image](https://user-images.githubusercontent.com/125276800/219000474-35f24fb6-aa78-4f20-b7fc-06f1d1473b31.png).

Il est n�cessaire de filtrer, les lignes afin de trouver un r�sultat concluant, pour cela on utilise la commande __���grep �138.66.89.12� | grep �pass� access.log��__. Cette commande, permet de trouver le mot de passe dans les log.
Pour rappels, l�adresse IP est celle qu�on � trouver gr�ce � l�historique des commandes qui correspond donc � l�attaquant. 

Cela ressort le r�sultat suivant�:




Nous avons maintenant le mot de passe, nous pouvons donc exploiter le zip trouver pr�c�demment.
Pour rappel, le fichier se situe dans __/opt/leak__

![image](https://user-images.githubusercontent.com/125276800/219037555-f0a1fa09-3e0a-41bf-8a43-4c4e27b97f94.png).

Nous pouvons maintenant d�compresser le fichier, cela nous ressors une erreur de droit. Il est n�cessaire de le d�placer pour cela on ex�cuter la commmande suivante __���unzip bosh_cyber_tools.zip -d /home/b0osh/����__�. On d�place le fichier, dans __/home/b0osh__, il est n�cessaire de taper le mot de passe que nous avons trouv� dans les log.

Le fichier d�zipper contient�un fichier .txt�:
