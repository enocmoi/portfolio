---
title: Station de santé connectée (prototype)
publishDate: 2020-03-02 00:00:00
img: /assets/stationsante/stock-station.png
img_alt: Stock image station santé
description: |
  Projet M1 : Création d'une station de santé connectée capable d'envoyer un diagnostic.
tags:
  - Dev
  - Santé
  - Système embarqué
---

## Introduction 

### Description du projet

> Station de santé connectée : <i>Yealth</i>

Le projet consiste à la prise de constantes liée à la santé de l’utilisateur, en alliant simplicité et accessibilité. 

Ce projet est réalisé à l’occasion de la 4ème année du cycle ingénieur de l’ISEN Yncréa Ouest Brest de l’année 2021-2022. 

La création d’une station de santé sous forme de système embarqué est essentielle car facilite la vérification des données contrôlées lors d’un examen médical simple, et permet de ménager le travail du personnel de santé ou même de rendre accessible ce service aux personnes à leur domicile. 

Par le biais de capteurs connectés à une <b>Raspberry Pi</b>, et d’une interface simple d’utilisation, l’utilisateur peut prendre sa température, son pouls ainsi que sa tension. Configuré, l'appareil permet de stocker des données et les envoyer à un professionnel de santé.

### Solutions matérielles

#### <b>Raspberry Pi4</b>

On utilise la Raspberry Pi 4 comme carte de développement.

<img src="/assets/stationsante/raspberry.jpg" alt="Raspberry pi 4" />

<i>Raspberry Pi4</i>

#### <b>Capteurs</b>

##### Capteur de température / humidité

Le capteur DHT22 est utilisé comme capteur de température corporelle en l’utilisant sous les aisselles. 

<img src="/assets/stationsante/DHT22.png" alt="DHT22" />

<i>DHT 22</i>

##### Fréquencemètre

Le capteur de fréquence cardiaque est utilisé. 

<img src="/assets/stationsante/frequencemetre.jpg" alt="frequencemetre" />

<i>Capteur de fréquence cardiaque</i>

Le capteur est normalement conçu pour une utilisation sur Arduino. 
Associé à un convertisseur adafruit MCP3008 qui permet la lecture du signal analogique du capteur, il peut être utilisé sur la carte Raspberry Pi.

<img src="/assets/stationsante/MCP3008.jpg" alt="MCP3008" />

<i>MCP3008</i>

##### Tensiomètre

Le tensiomètre est utilisé pour calculer la tension.

<img src="/assets/stationsante/tensiometre.jpg" alt="tensiometre" />

<i> BREAKOUT003 </i>

#### <b>L'écran tactile</b>


Ecran assez petit pour que la station soit aisément transportable. Fonction tactile pour rendre la station plus accessible.

<img src="/assets/stationsante/ecran.jpg" alt="ecran" />

<i>Ecran LCD HDMI Tactile 4 pouces.</i>

#### <b>Boitier imprimé en 3D</b>

La maquette du boitier est réalisé sur Fusion 360.

<img src="/assets/stationsante/boitier-3d.png" alt="boitier" />

<i>Maquette de boitier.</i>

L'impression 3D est ensuite réalisée.

### Solutions logicielles

#### <b>IHM</b>

Codé en JavaFX, l'IHM permet d'afficher les résultats obtenus par les capteurs.
Les logiciels Apache Netbeans et SceneBuilder ont été utilisés pour le développement.


<img src="/assets/stationsante/IHM.png" alt="IHM" />

<i>IHM</i>

Le graphique de température à deux lignes rouges permettant à l'utilisateur de comprendre directement si sa température corporelle est bonne. 
De même pour la fréquence cardiaque au repos.

#### <b>Python</b>

Après hésitation sur le choix du langage, Python est utilisé car la complexité du programme n'est pas élevé, alors il se réalise assez rapidement avec Python. 
De plus, ce langages permet l'utilisation de différences bibliothèques et modules simples d'utilisation. 

#### <b>Tkinter</b>

Tkinter est une interface graphique python qui permet de rendre le programme executé plus attrayant. On utilise le widget <i>canevas</i> qui permet d'afficher les graphes des données des capteurs en fonction du temps.

#### <b>Intelligence artificielle</b>

Une intelligence artificielle permettant de déterminer si le patient est malade ou non fut réalisée en Python. L’algorithme se base sur quelques données de température affiliée à un BPM, dont on donne le caractère “sain” ou “malade” en fonction des valeurs calculées. Puis, on demande de prédire pour une valeur combinée de température, et BPM, en fonction des données précisées si le patient est malade. 

Cependant ce procédé ne s’avère pas précis, alors une autre solution était de procéder par régression linéaire. Une ébauche fut réalisée, mais celle-ci n’était pas exploitable (par manque de temps précédant le rendu du projet).

#### <b>Configuration de la carte électronique</b>

La configuration de la Raspberry Pi 4 nécessite de flasher une carte SD pour le système d'exploitation Linux. 

### Architecture du projet

L’ensemble des capteurs comporte un code en python propre à chacun pouvant être relié ensemble sur la même interface. Cette architecture logicielle, ne représente pas tous les codes déjà implémentés dans la Raspberry PI, pouvant être modifiée en fonction de son utilisation. 

<img src="/assets/stationsante/architecture.png" alt="architecture" />

<i>Architecture logicielle et matérielle du projet.</i>


### Résultats 

Après assemblage des élements, voilà la station de santé prototype obtenue :

<img src="/assets/stationsante/station-finale.png" alt="station finale" />

<i>Prototype de station de santé connectée.</i>

### Conclusion et finalité de ce premier projet.

La station de santé connectée “Yealth” allie système embarqué et système permettant de consulter son état de santé. 

Malgré les diverses idées énoncées, quelques-unes n’ont pas été réalisables, suite à des difficultés ou un manque de temps. 

Finalement, la station de santé connectée se compose de deux capteurs, un thermomètre et un fréquencemètre. 

Seulement, ce dernier ne fonctionne pas comme prévu et affiche des valeurs qui sont impossibles (un pouls qui passe de 30 BPM à plus de 200 BPM en une seconde). 

La température prise sous l’aisselle respecte les mesures et les résultats attendus (entre 36,1°C et 37,8°C). 

La station se présente sous la forme d’une boîte rouge créée en impression 3D. 

Elle comporte deux trous laissant passer les capteurs. 
Elle se dirige à l’aide d’un écran tactile qui laisse apparaître une interface facile d’utilisation. 
La station est embarquée grâce à un branchement sur batterie externe, ce qui permet de pouvoir la déplacer aisément sans encombre.  

Sur l’interface sont traitées les données de la prise de température et du pouls, la date et l’heure. Deux lignes horizontales permettent la visualisation claire des valeurs correctes ou non, et un code couleur indique si l’utilisateur est malade ou sain. 

Les données devaient être stockées et envoyées à une intelligence artificielle, mais ces champs n’ont pas été remplis par manque de temps.
