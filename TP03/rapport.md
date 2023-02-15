## Introduction
Le site Bosch-Cyber a subi une compromission, lors de laquelle un attaquant a utilis� diff�rents outils pour s'infiltrer dans le syst�me. Notre objectif est de d�terminer quelles informations l'attaquant a extraites du site et de d�terminer des recommandations afin que cela arrive plus. Cette mission sera effectu�e par Pasquet Juliette.
## M�thodologie 

La premi�re chose � faire est d�acc�der au log afin de d�terminer l�heure et la date de l�attaque, les fichiers sont disponibles dans __/var/log__. Nous pouvons supposer que l�attaque � commenc� le 3 septembre dans la matin�e. Les logs permettent de d�terminer que plusieurs connexions ont �chou� le 3 septembre � partir de 12h-13h. Ci-dessous le fichier ��error.log���:

![image](https://user-images.githubusercontent.com/125276800/218995982-150799f8-de7f-4090-8629-d80dd8029c87.png)
Le premier message d'erreur indique que la poign�e de main SSL a �chou� en raison d'une mauvaise cl� partag�e entre le client et le serveur. Cela peut indiquer un probl�me avec le certificat SSL install� ou avec la configuration du serveur web.
Le deuxi�me message d'erreur indique que le serveur web a re�u une demande HTTP GET pour le fichier __"info.php"___, mais que le script PHP n'a pas r�ussi � r�cup�rer les valeurs des en-t�tes http.
De plus, il est n�cessaire d�acc�der au fichier __��access.log��__
![image](https://user-images.githubusercontent.com/125276800/219000474-35f24fb6-aa78-4f20-b7fc-06f1d1473b31.png)

