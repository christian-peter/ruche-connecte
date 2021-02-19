# Outil de suivi de ruchers
Ce projet est en cours de développement. Tous les éléments décrits dans le synoptique ne sont donc pas fonctionnels à ce jour.

Certains éléments ont été développés par des apprentis ingénieurs suivant la formation "électronique et informatique industrielle" à 
[Polytech Nice Sophia](http://unice.fr/polytechnice/fr). 

L'objectif consiste à relever différents paramètres (masse, vibrations, température et hygrométrie interne à la ruche, données météorologiques) pour les rendre accessibles à distance via des périphériques tels qu'un ordinateur, une tablette, un smartphone ....
 
Afin de remplir ces différentes fonctions, le système global à été scindé en plusieurs blocs. Les fonctions des trois premiers modules, qui doivent être autonomes en énergie, sont les suivantes :
-relever la masse de la ruche ;
-mesurer les grandeurs internes (températures, hygrométrie, vibrations, ....) ;
-mesurer les données météorologiques (température, hygrométrie, vent, pluie, ensoleillement, ...).

Les données relevées par ces modules sont transmises via un réseau sans fils, dont la portée doit être de plusieurs kilomètres, à une passerelle dont la fonction consiste à enregistrer ces mesures dans une base de données. Ces données sont alors rendues accessibles via un serveur "web". 

![synoptique](/Images/croquis_rucher.png)
