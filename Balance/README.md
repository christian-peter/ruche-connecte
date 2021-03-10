# LA BALANCE

## Préambule

Pour la partie mécanique qui ne sera pas développée ici (mis à part quelques photos de prototypes), je me suis inspiré des balances trouvées dans le commerce dont les capteurs de force sont de ce type :

![capteur 1](/Balance/Images/load_beam.png)

Mon premier prototype avec un capteur n'était pas assez stable à mon gout. J'en ai donc réalisé 
[d'autres](/Balance/Images/proto_meca.png) avec 2 capteurs puis 4 capteurs. 

J'ai aussi testé les capteurs de pèse personnes.

![capteur 2](/Balance/Images/pese_personne.png)

L'électronique est donc conçue pour s'adapter à ces différentes configurations. 

## Fonctionnement

Après mise sous tension, le module entre en mode sommeil pour réduire la consommation au maximum.
Il se réveille à intervalles réguliers. Il mesure alors la masse et envoie les données (avec un code d'erreur CRC) via un réseau LoRa puis attend un accusé de réception. A réception d'un accusé de réception valide, le module repasse en mode sommeil ; si l'accusé de réception n'est pas valide, les données sont renvoyées (au maximum 'send_max_tries' fois). Sans accusé de réception au bout de 'WAIT_TIME_FOR_ACKNOWLEDGE' le module passe en mode sommeil.

La durée de sommeil est paramétrable et différenciée entre jour et nuit. La période 'jour' débute 1 heure avant le début du jour et s'achève 1 heure après la fin du jour.


Le module peut aussi être réveillé manuellement par un appui bref sur la touche capacitive gauche ; un menu à dérouler avec la touche capacitive droite permet alors de sélectionner différentes actions ou d'afficher différentes informations :

* création d'une visite (dans la base de données),
* mise en sommeil,
* nourrissement 50/50,
* nourrissement 70/30,
* nourrissement candy,
* modification matérielle,
* mesure de la masse,
* informations batterie,
* informations version et numéro de balance,
* informations LoRa,
* informations température capteurs.

Une fois le choix validé par la touche capacitive gauche, les données sont envoyées ou les informations affichées et le module retourne au mode sommeil.

## Calibration et réglages

Un appui maintenu sur la touche capacitive gauche permet d'accéder à la procédure de calibration ou au réglage des paramètres.

### Calibration

Avant la première utilisation, il faut calibrer la balance. Il faut suivre les indications affichées sur l'écran LCD.

### Réglages

Un certain nombre de paramètres sont modifiables en se connectant au module en Bluetooth avec un terminal série : par exemple 
[Serial Bluetooth Terminal](https://play.google.com/store/apps/details?id=de.kai_morich.serial_bluetooth_terminal&hl=fr&gl=US).

Les paramètres sont regroupés par famille et sont les suivants :

* réglages généraux :
    - intervalle de mesure le jour,
    - intervalle de mesure la nuit,
    - intervalle de mesure pour visite,
    - nombre maximum de transmission,
    - numéro de la balance,
* réglages LoRa :
    - fréquence, 
    - facteur d’étalement de spectre (7 à 12; 12 <=> portée maximale, mais autonomie plus faible),
    - bande passante,
    - puissance d'émission (7 à 15 dB <=> portée maximale, mais autonomie plus faible),
* dérive des capteurs en température,
* initialisation de la mémoire EEPROM (reset des paramètres de calibration et autres réglages).

exemple vidéo : [mp4](http://rucher.polytech.unice.fr/ruche-connecte/videos/reglages.mp4) 
[wmv](http://rucher.polytech.unice.fr/ruche-connecte/videos/reglages.wmv)
<!-- 
ffmpeg -i /tmp/reglages.mp4 -vf scale=960:-1  -an output2.mp4
https://www.linuxtricks.fr/wiki/ffmpeg-la-boite-a-outils-multimedia
https://tuxicoman.jesuislibre.net/2017/01/changer-la-resolution-dune-video-avec-ffmpeg-sous-linux.html
-->

## Electronique

![schema-scale](/Balance/ESP_LoRa_Scale_V1.60.png)

Les schémas et PCB sont dessinés avec [eagle](https://www.autodesk.com/products/eagle/overview?plc=F360&term=1-YEAR&support=ADVANCED&quantity=1) :

* [schéma](/Balance/ESP_LoRa_Scale_V1.60.sch)
* [pcb](/Balance/ESP_LoRa_Scale_V1.60.brd)
* [gerber](/Balance/gerber/ESP_LoRa_Scale_V1.60.zip)
* [liste du matériel](/Balance/ESP_LoRa_Scale_V1.60.csv)

L'[antenne 'UCA'](http://users.polytech.unice.fr/~ferrero/recherche_UCAboards.html) 
LoRa, déssinée par Fabien Ferrero, est supportée par le circuit imprimé. 
Pour une efficacité maximale de l'antenne, ce dernier doit être réalisé en FR4 avec une épaisseur de 0,8mm.

Il est aussi possible de connecter une antenne externe via un connecteur UFL, la piste reliée à l'antenne 'UCA'
doit alors être coupée.  

![pcb-scale](/Balance/Images/pcb-scale.png)

Cette carte peut recevoir 1, 2 ou 4 capteurs de force. Le convertisseur NAU7802 U4 et les composants associés
ne sont utiles que pour l'utilisation de 4 capteurs ; le pont SJ7 doit alors être court-circuité.
Avec 1 ou 2 capteurs on laisse SJ7 ouvert pour réduire la consommation et on utilise les entrées du convertisseur U2.
Un capteur unique doit être connecté sur les entrées 'A', les entrées 'B' étant reliées à la masse.

Le connecteur 'JP3' permet de connecter un capteur de température dans le but de compenser les dérives thermiques des capteurs de force. Pour l'instant seul est supporté le capteur de type 18B20. On pourrait aussi utiliser des capteurs de la famille DHT11,
permettant d'obtenir aussi une mesure d'hygrométrie mais la partie software devra être modifiée en conséquence.

## Logiciel

L'archive est à extraire dans le dossier 'projet' du l'IDE arduino.

* [ESP_LoRa_Scale_V0.34.zip](/Balance/ESP_LoRa_ScaleV0.34.zip)

Hormis la mesure de tension de la batterie la partie 'fuel gauge' reste à développer.

## Mesures

à venir

## Prototypes

aussi


