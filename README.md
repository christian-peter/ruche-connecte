# Outil de suivi de ruchers
Ce projet est en cours de développement. Tous les éléments décrits dans le synoptique ne sont donc pas fonctionnels à ce jour.

Certains éléments ont été développés par des apprentis ingénieurs suivant la formation "électronique et informatique industrielle" à 
[Polytech Nice Sophia](http://unice.fr/polytechnice/fr). Une page spécifique à ce projet est disponible [ici](http://rucher.polytech.unice.fr/).  

L'objectif consiste à relever différents paramètres (masse, vibrations, température et hygrométrie interne à la ruche, données météorologiques) pour les rendre accessibles à distance via des périphériques tels qu'un ordinateur, une tablette, un smartphone ....  
 
Afin de remplir ces différentes fonctions, le système global à été scindé en plusieurs blocs. Les fonctions des trois premiers modules, qui doivent être autonomes en énergie, sont les suivantes :

* relever la masse de la ruche ;
* mesurer les grandeurs internes (températures, hygrométrie, vibrations, ....) ;
* mesurer les données météorologiques (température, hygrométrie, vent, pluie, ensoleillement, ...).

Les données relevées par ces modules sont transmises via un réseau LoRa, dont la portée doit être importante, à une passerelle dont la fonction consiste à enregistrer ces mesures dans une base de données. Ces données sont alors rendues accessibles via un serveur "web". 

![synoptique](/Images/croquis_rucher.png)

L'électronique a été développée autour d'un microcontrôleur [ESP32](http://esp32.net/) d'Espressif Systems, basé sur l'architecture Xtensa LX6 de Tensilica, intégrant la gestion du Wi-Fi et du Bluetooth et d'un modem LoRa [RFM95](https://www.hoperf.com/modules/lora/RFM95.html).

Elle a été conçue avec un soucis de polyvalence pour permettre différentes configurations mais aussi pour faciliter les évolutions ou optimisations futures.

C'est la cas notamment pour l'alimentation des différents modules. La tension d'alimentation des circuits précités est de 3.3V (maxi 3.6V). Il peuvent donc être alimentés directement par des batteries lithium-fer-phosphate (LiFePO<sub>4</sub>). Afin de pouvoir utiliser également d'autres sources d'énergie (batteries Lipo, Liion, plomb, .... ) les cartes sont équipées d'un régulateur de tension (MIC5239).

![alimentation](/Images/alim.png)

Une carte batterie spécifique peut être connectée via les connecteurs JP2 et JP3, elle peut être équipée d'une batterie LiFePO<sub>4</sub> connectée entre la masse et le signal '3.3_BAT'.

En l'absence d'alimentation sur l'entrée '+12V', le transistor Q4 est passant alimentant ainsi les éléments connectés à 'Vcc' à partir de la batterie LiFePO<sub>4</sub>. 

Si la carte est alimentée par l'entrée '+12V', les éléments connectés à 'Vcc' sont alimentés par le régulateur MIC5239. Le transistor Q4 est bloqué car le régulateur ne doit pas charger la batterie. L'entrée '+12V' peut recevoir des tensions comprises entre 3.6V et 30V. 
<!-- Les cartes sont généralement prévues pour recevoir un régulateur en boîtier :

* soit SOT-223,
* soit MSOP.-->

Dans le but d'économiser l'énergie, les périphériques ne sont alimentés par 'V+' que lorsque le transistor Q1 est passant (cad lorsque la sortie 'MEASURE' de l'ESP32 est au niveau haut).


# La réalisation 

* la balance
* le module interne
* le module météorologique
* la carte batterie
* la passerelle (gateway)

