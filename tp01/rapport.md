Pour garantir la sécurité de l'utilisation d'une clé USB trouvée, il est nécessaire de l'analyser en utilisant des outils. 
Les photos se trouvent dans le répertoire "IMG". 
Le premier outil utilisé est "Hashtab", qui permet de déterminer le hachage utilisé, à savoir CRC32, MD5 et SHA-1, comme indiqué sur la photo dans le répertoire.

Le deuxième outil est "Autopsy", une application graphique qui facilite l'analyse forensique des systèmes de fichiers. 
Il est utilisé sur Kali, mais nécessite l'installation de JavaScript, que je n'ai pas pas réussi à installer (voir photo 2). 
Avant d'utiliser les outils, il est nécessaire de copier les données de la clé USB à l'aide de l'outil "dd" pour éviter d'éventuelles pertes de données.

L'autre outil à utiliser est "Binwalk" qui est un outil Python intégré qui est utilisé pour analyser, désosser et extraire des images de micrologiciel.
 Beaucoup de gens qui jouent aux CTF utilisent cet outil pour analyser les fichiers qu'ils trouvent. ( CF: photo binwalk).

Nous pouvons voir qu'il y a différents photos d'anamiaux ( cf : photo), pour cela nous avons utilisé la commande "photorec" ( cf : photorec).












