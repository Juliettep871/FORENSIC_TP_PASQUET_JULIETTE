## Introduction


Un policier a trouvé une clé USB sur un parking. Afin de déterminer si cette clé est malveillante, il est nécessaire de l'analyser à l'aide d'outils spécialisés.
Les photos illustrant les différentes étapes se trouvent dans le répertoire "IMG". Avant d'utiliser les outils, il est important de copier les données de la clé USB en utilisant l'outil "dd" pour éviter toute perte de données.
 
## Analyse 

La première étape est d’analyser les métadonnées.

Pour cela, le premier outil utilisé est "SHA256sum". Cette commande permet de déterminer la somme de contrôle SHA256 (voir photo : SHA256sum). Comme le montre la photo, le résultat est le même.


![image](https://user-images.githubusercontent.com/125276800/218699761-22c01409-0150-491d-a079-e27c9a0f0e05.png)

Avant de tenter de trouver les fichiers qui se trouvent sur la clé, il est nécessaire d'analyser plus en détail le disque. Pour cela, on utilise la commande "fdisk -l" pour lister toutes les partitions (voir photo : fdisk).  

Puis, il est important de connaitre la taille du disque ainsi que les droits du fichier on utilise la commande « exiftool USB_Image » (voir image : exiftool)
Le deuxième outil est "Autopsy", une application graphique qui facilite l'analyse forensique des systèmes de fichiers. Il est utilisé sur Kali, mais nécessite l'installation de JavaScript, que je n'ai pas réussi à installer (voir photo 2).

L'autre outil à utiliser est "Binwalk", qui est un outil Python intégré utilisé pour analyser, désosser et extraire des images de micrologiciel. En tapant une commande, nous pouvons analyser la clé (voir photo : binwalk). La capture d'écran montre qu'il y a différents fichiers, notamment des images de format « png » et « jpeg ». 

![image](https://user-images.githubusercontent.com/125276800/218701486-d7336de3-ff54-44c3-998c-971e37bf5e3f.png)

Il faut maintenant récupérer les documents. Pour cela, j'ai utilisé plusieurs outils comme "Scalpel". Malheureusement, cet outil ne permet pas de récupérer toutes les données (voir la photo : Scalpel). En effet, on ne peut accéder qu'aux fichiers ayant l'extension "png".

J’ai alors utilisé l’outil "photorec". La commande est la suivante : "photorec USB_IMAGE". La commande ouvre une fenêtre où il est nécessaire de sélectionner le disque que l'on souhaite analyser, ainsi que le système de fichiers. L'analyse est lancée et le résultat montre 3 fichiers PNG, 2 fichiers JPG et 1 fichier TXT (voir photos : photorec 2&3). Les fichiers sont situés dans deux répertoire qui se nomme « recup_dir1 et recup_dir2 » (voir photo : recup_dir »), crée par la commande.

![image](https://user-images.githubusercontent.com/125276800/218700611-c88d4fa2-7689-42a5-91b3-7b2bd6fd3a6e.png)



Voici les différentes photos récupérées (voir photo : resultat photo). Nous pouvons voir des photos de plusieurs animaux, un fichier .ini, deux flag.
![image](https://user-images.githubusercontent.com/125276800/218700061-3070b806-ee18-4f69-a058-56ed906d73cb.png)

De plus, il y a un texte qui est le suivant : "bosch{1MAG3}". Le fichier .ini contient le message suivant : "[Trash info] path=secret.png deletionDate=2023-02-10T22:21:51". On pourrait en déduire que l'action a été réalisée le 10 février 2023 par un individu qui se prénomme "Bosch" (preuve donnée dans les images f0016520.png et f0040392.png).

![image](https://user-images.githubusercontent.com/125276800/218700731-1b42ec01-da4a-4d83-919a-760250ee6280.png)

De même, il serait intéressant d'utiliser Wireshark, qui est un analyseur de protocole de réseau pouvant être utilisé pour examiner les données réseau sur la clé USB



## Conclusion :

Pour donner suite à l'analyse de la clé USB, il a été déterminé qu'elle était sécurisée et ne présentait aucun risque. Ainsi, nous sommes en mesure de la brancher sur un ordinateur.









