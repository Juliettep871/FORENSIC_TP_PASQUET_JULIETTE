## Introduction
Le site Bosch-Cyber a subi une compromission, lors de laquelle un attaquant a utilisé différents outils pour s'infiltrer dans le système. Notre objectif est de déterminer quelles informations l'attaquant a extraites du site et de déterminer des recommandations afin que cela arrive plus. Cette mission sera effectuée par Pasquet Juliette.
## Méthodologie 

La première chose à faire est d’accéder au log afin de déterminer l’heure et la date de l’attaque, les fichiers sont disponibles dans __/var/log__. Nous pouvons supposer que l’attaque à commencé le 3 septembre dans la matinée. Les logs permettent de déterminer que plusieurs connexions ont échoué le 3 septembre à partir de 12h-13h. Ci-dessous le fichier « error.log » :

![image](https://user-images.githubusercontent.com/125276800/218995982-150799f8-de7f-4090-8629-d80dd8029c87.png)
Le premier message d'erreur indique que la poignée de main SSL a échoué en raison d'une mauvaise clé partagée entre le client et le serveur. Cela peut indiquer un problème avec le certificat SSL installé ou avec la configuration du serveur web.
Le deuxième message d'erreur indique que le serveur web a reçu une demande HTTP GET pour le fichier __"info.php"___, mais que le script PHP n'a pas réussi à récupérer les valeurs des en-têtes http.
De plus, il est nécessaire d’accéder au fichier __« access.log »__
![image](https://user-images.githubusercontent.com/125276800/219000474-35f24fb6-aa78-4f20-b7fc-06f1d1473b31.png)

