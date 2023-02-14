Pour garantir la sécurité de l'utilisation d'une clé USB trouvée, il est nécessaire de l'analyser en utilisant des outils. 
Les photos montrant les différentes étapes se trouvent dans le répertoire "IMG". 

Le premier outil utilisé est "SHA256sum", la commande permet de déterminer la somme de contrôle SHA256 ( cf photo: SHA256sum). Comme nous le prouve la photo, le résultat est le même.
Avant d’essayer de trouver les fichiers qui sont sur la clé, il est nécessaire d’analyser plus en détail le disk. Pour cela, on utilise la commande « fdisk -l » afin de lister toutes les partitions
 ( cf photo : fdisk).

Le deuxième outil est "Autopsy", une application graphique qui facilite l'analyse forensique des systèmes de fichiers. 
Il est utilisé sur Kali, mais nécessite l'installation de JavaScript, que je n'ai pas pas réussi à installer (voir photo 2). 
Avant d'utiliser les outils, il est nécessaire de copier les données de la clé USB à l'aide de l'outil "dd" pour éviter d'éventuelles pertes de données.

L'autre outil à utiliser est "Binwalk" qui est un outil Python intégré qui est utilisé pour analyser, désosser et extraire des images de micrologiciel. En tapant une commande, nous pouvons analyser la clé( CF: photo binwalk). La capture nous montre qu’il y a divers fichiers comme notamment des images.

Il faut maintenant récupérer les documents, pour cela on utilise l’outil « photorec ». La commande est la suivante : « photorec USB_IMAGE ». La commande, ouvre une fenêtre il est nécessaire de sélectionner le disque qu’on veut, le système de fichiers. L’analyse se lance et remonte le résultat suivant : 3 png, 2 jpg et 1 txt (cf photo : photorec 2&3).

Voici les différentes photos récupérer (cf photo : resultat photo), nous pouvons voir des photos de plusieurs animaux, 1 fichier ini et un texte qui est le suivant « bosch{1MAG3 }». Le fichier .ini contient le message suivant « [ Trash info] path=secret.png deletionDate=2023-02-10t22 :21 :51 ». On pourrai en déduire que l’action à été réalisé le 10 février 2022 par un individu qui se prénomme « Bosch » ( preuve donnée dans l’image f0016520.png et f0040392.png »

De même, il serai intéressant d'utiliser Wireshark qui  est un analyseur de protocole de réseau qui peut être utilisé pour examiner les données réseau sur la clé USB.













