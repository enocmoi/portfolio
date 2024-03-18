---
title: Tabouret suiveur de personnes (prototype)
publishDate: 2020-03-02 00:00:00
img: /assets/tabouret/stock-tabouret.jpg
img_alt: Stock image tabouret
description: |
  Projet M2 : Création d'un tabouret suiveur de personne.
tags:
  - Dev
  - Intelligence Artificielle
  - Système embarqué
---

## Introduction 

### Description du projet

> Tabouret suiveur de personne assisté par intelligence artificielle

Le projet consiste en l’invention et la conception d’un projet de tabouret suiveur de personne, qui suit une personne lorsqu’un signal visuel lui est donné. 

Le tabouret est équipé d’intelligence artificielle. 

Le système utilise une caméra pour détecter le signal visuel et suit la personne en ajustant la position et la vitesse du tabouret en temps réel.

Ce projet est réalisé à l’occasion de l’année M2 du cursus intelligence artificielle du cycle ingénieur de l’ISEN Yncréa Ouest Brest de l’année 2022-2023.

En utilisant des algorithmes d’apprentissage automatique pour apprendre à reconnaître les signaux visuels donnés par la personne, le tabouret la suit avec précision. 


### Solutions matérielles

#### <b>Jetson Nano</b>

On utilise la Jetson Nano comme carte de développement.
L’alimentation de la Jetson Nano est faite via une batterie externe, afin d’avoir un tabouret dont la technologie est embarquée.

<img src="/assets/tabouret/jetson.jpg" alt="Jetson Nano" />

<i>Jetson Nano</i>

#### <b>Carte Brushless motor</b>

La carte brushless motor, comme le suggère le nom, contrôle le moteur. 
Afin de relier la batterie à cette carte d’asservissement, un pont diviseur de tension à été réalisé, en sachant que la batterie a une entrée de 36V et 4,3A, et que la sortie doit être de 10-15V pour 2-4A. 

<img src="/assets/tabouret/montage-brushless.png" alt="Carte brushless motor" />

<i>Figure du montage prévu</i>

Il faut noter que faire un pont diviseur de tension dans lequel on fait passer 3A mérite une attention particulière au choix du fil pour le montage.

Il faut également penser à relier la batterie à des interrupteurs afin d’éviter des arcs électriques pendant la manipulation.

Il faut également abaisser la tension : pour se faire, on utilise un convertisseur AC/DC qui accepte une entrée allant jusqu’à 42V.

#### <b>Caméra</b>

Afin de capter les signes pour activer le tabouret, une caméra fût choisie : en l'occurrence la caméra OAK-1, développée par OpenCV.

<img src="/assets/tabouret/oak-1.jpg" alt="OAK 1" />

<i>Caméra OAK 1</i>

#### <b>Impression 3D</b>

La maquette du boitier est réalisé sur Solidworks. Il est ensuite imprimé sur imprimante 3D Ultimaker.
On réalise 3 boîtes : une pour la Jetson Nano, une autre pour le contrôle des roues et une dernière pour la carte batterie.

<img src="/assets/tabouret/3d-jetson.png" alt="boitier jetson" />

<i>Maquette de boitier pour la Jetson Nano.</i>

<img src="/assets/tabouret/3d-brushless.png" alt="boitier brushless" />

<i>Maquette de boitier pour la carte Brushless motor (contrôle des roues).</i>

<img src="/assets/tabouret/3d-batterie.png" alt="boitier batterie" />

<i>Maquette de boitier pour la batterie.</i>


<img src="/assets/tabouret/3d-jetson.png" alt="boitier jetson" />

<i>Maquette de boitier pour la Jetson Nano.</i>


#### <b>Roues et chassis</b>

Les deux roues provienne d'un jeux type "hoverboard"

<img src="/assets/tabouret/overboard.jpg" alt="overboard" />

<i>Maquette de boitier pour la batterie.</i>

Le choix des matériaux s’est fait pour le prototype de manière à ne pas avoir un coût élevé, et pouvoir fixer facilement tous les éléments ensemble.
On utilise pour le châssis une planche de bois type cailleboti de terrasse. Cette structure n’est prévue que pour le prototype mais permet d’avoir une surface assez large et solide pour poser tous les éléments du tabouret suiveur de personne.


### Solutions logicielles

#### <b>Python</b>

Le langage choisi est Python car il a à sa disposition de nombreuses librairies permettant la création et le déploiement de modèles de machine learning (Tensorflow, PyTorch). 

#### <b>Intelligence artificielle</b>

##### 1. Résumé
On a utilisé deux modèles d'intelligence artificielle en vision par ordinateur. 

Le premier modèle est un détecteur d'êtres humains, tandis que le deuxième modèle est chargé de détecter les signes de la main émis par les personnes détectées, afin de permettre au tabouret de se déplacer vers elles ou de les suivre. 

Les deux modèles ont été entraînés en utilisant la base de données YOLOv8, bien que les données sources pour chacun d'eux diffèrent.

##### 2. Acquisition des données

Deux ensembles de données distincts : un pour la détection des êtres humains et un autre pour la détection des signes de la main.

 - <b>Dataset êtres humains</b> : On a utilisé le dataset COCO sur lequel le modèle YOLOv8 a été entraîné. Le dataset COCO permet la détection de 80 objets différents, y compris les êtres humains, avec une précision assez élevée.

 - <b>Dataset signes de la main</b> : On a créé notre propre ensemble de données avec une acquisition de données spécifique pour capturer les différentes positions et gestes de la main que nous souhaitons détecter.

##### 3. YOLOv8

YOLOv8 est un modèle de vision par ordinateur développé par Ultralytics et publié en janvier 2023. Il est considéré comme l'état de l'art (SOTA) dans ce domaine. On a choisi d'utiliser YOLOv8 comme base pour nos modèles, ce qui nous a permis de bénéficier des dernières avancées et des performances élevées offertes par ce modèle.

#### <b>Configuration de la carte électronique</b>

L’OS de la Jetson Nano est la version 18.04 d’Ubuntu. Il à été modifié par Nvidia pour être compatible avec le hardware de la Jetson notamment son GPU propriétaire (librairies CUDA pré-installées). La carte SD a donc été flashée avec le Jetpack SDK 4.6 fourni par Nvidia.  

#### <b>Reconnaissance faciale</b>

L'utilisation de la caméra OAK-1 permet la reconnaissance faciale. D'abord, la bibliothèque <i>depthAI</i> est utilisée. Un script est développé permettant de capturer en temps réel les images de la caméra et les utiliser dans les algorithmes de reconnaissance.

On a ensuite choisi Yolov8Pose qui propose des modèles légers permettant d'estimer la position d'un squelette sur une personne. Avec une fréquence d'image de 10 FPS, le modèle est performant.

### Fonctionnement général du système

Le fonctionnement de notre système est basé sur la détection des personnes dans les images capturées par la caméra, ainsi que la détection d'un geste de la main situé au-dessus des épaules. 

Lorsque ce signe est détecté, notre système doit localiser la personne qui l'a effectué d'une image à l'autre. 

Cette localisation permet ensuite de calculer les instructions à envoyer aux roues afin que le tabouret puisse suivre cette personne.

> En résumé, le système consiste en une étape de détection des personnes, une étape de détection du geste de la main et une étape de suivi de la personne pour assurer le déplacement du tabouret en fonction de ses mouvements.

Ce système est structuré en deux programmes interagissant entre eux :

#### 1. Acquisition d'images et analyse grâce à l'intelligence artificielle

On récupére les images et les envoyons au modèle pour obtenir les points clés du squelette. Puis les bounding box ainsi que les positions des épaules et des mains sont récupérées.
On détecte si l'une des personnes détectées lève la main en évaluant la position des mains par rapport aux épaules. Si tel est le cas, le tabouret suit cette personne.

Pour assurer le suivi d'une personne d'une image à l'autre, on utilise la distance entre les boîtes englobantes. La bounding box sur la frame "n" ayant la plus faible distance par rapport à la bounding box suivie sur la frame "n-1" correspond à la même personne. 

Ensuite, on calcule les actions que le tabouret doit effectuer pour suivre la personne. 
L'objectif est de placer la bounding box de la personne au centre de l'image. On effectue des calculs basés sur la taille de la bounding box et la taille de l'image pour déterminer les actions à effectuer sur les roues afin de centrer la personne dans l'image. 

On détermine deux valeurs comprises entre -3 et 3. 
- La première valeur détermine la vitesse que le tabouret doit prendre : en positif, il avance ; en négatif, il recule ; et à 0, il s'arrête. 
- La deuxième valeur détermine la direction à prendre : en négatif, il tourne à droite ; en positif, il tourne à gauche ; et à 0, il continue tout droit.

On  enregistre ces valeurs dans un fichier qui sera lu par le deuxième programme.

#### 2. Interprétation des informations pour contrôler les roues

Dans ce programme, l'objectif est de récupérer les données écrites par le premier système et de les appliquer aux roues. 
Pour cela, on utilise des changements GPIO (General Purpose Input/Output) et PWM (Pulse-Width Modulation). 

### Architecture du projet

Au final, le schéma du tabouret suiveur de personne est tel :

<img src="/assets/tabouret/architecture.png" alt="architecture" />

<i>Architecture logicielle et matérielle du projet.</i>


### Résultats 

Après assemblage des élements, voilà la station de santé prototype obtenue :

<img src="/assets/tabouret/stock-tabouret.png" alt="Tabouret final" />

<i>Prototype de tabouret suiveur de personne.</i>

### Conclusion et finalité de ce projet.

Le tabouret suiveur de personne combine des algorithmes d'apprentissage automatique et une caméra. Il est capable de suivre une personne en temps réel, en ajustant sa position et sa vitesse.

Cela reste un prototype, mais les résultats sont prometteurs, et peuvent être une réelle solution pour les personne ayant des difficultés à se déplacer.