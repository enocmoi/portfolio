---
title: Détection et quantification du nombre de personnes dans une file d'attente
publishDate: 2020-03-02 00:00:00
img: /assets/filedattente/stock-file-d-attente.png
img_alt: Stock image file d'attente
description: |
  Projet Stage : Détection et visualisation du nombre de personnes dans une file d'attente.
tags:
  - Dev
  - IA
  - Computer Vision
---

## Introduction 

### Description du projet

> Détection et visualisation du nombre de personnes dans une file d'attente

Ce projet est réalisé à l’occasion d'un stage au sein de l'entreprise <i>STUnited</i> à Danang, Vietnam. 

Le problème qu'on aborde dans ce projet découle des expériences des clients et du personnel dans les files d'attentes de magasins, qui peuvent rapidement se retrouver pleines.
Pour les clients, les files d'attentes longues peuvent mener à leur impatience, et baisser ainsi leur satisfaction.
D'autre part, le personnel chargé de gérer ces files d'attentes peuvent se retrouver rapidement submergés par le travail. Ils doivent garantir un flux de clients fluide, répondre aux pics soudains de clients qui attendent.

La mission principale est de trouver une solution à ce problème en développant un système capable de détecter avec précision et d'alerter le personnel en cas de flux trop important de clients dans les files d'attentes.

On utilisera alors deux caméras, une avec une vision de côté et une avec une vision du dessus, permettant une meilleure précision.

### Solutions matérielles

#### <b>Caméra Jovision</b>

On utilise deux caméras de surveillance utilisées par notre client, qui utilise le protocole RTSP.

<img src="/assets/filedattente/camera.jpg" alt="Camera Jovision" />

<i>Caméra Jovision</i>

### Solutions logicielles

#### <b>Python</b>

On utilise Python 3.10, qui permettra facilement l'utilisation de bibliothèques pré-faites ainsi que de YOLO.

#### <b>Intelligence artificielle</b>

Pour ce projet on utilise une intelligence artificielle par computer vision, entrainée par un dataset.

##### 1. Dataset

On utilise le dataset COCO afin d'entrainer le modèle YOLOv8. Cela permet de détecter 80 objets différents, mais surtout des humains. 

##### 2. YOLOv8

Le choix du modèle fût suite aux performance qu'ils proposent.

<img src="/assets/filedattente/yolo-perf.png" alt="yolo performances" />

<i>Différent modèles YOLO et les performances associées.</i>

On a opté pour le modèle YOLOv8-Pose. On peut ainsi identifier l'emplacement de points spécifiques placé sur le squelette d'un humain, appelés <i>keypoints</i>. 
La sortie d'un modèle d'estimation de la pose comprend un ensemble de keypoints et leurs scores de confiance associés. 
Les emplacements des keypoints sont généralement représentés sous forme de coordonnées 2D ou 3D.

Il est à noter qu'on a choisi la version YOLOv8 Nano en raison de limitations matérielles, car mon ordinateur ne disposait pas d'un GPU, ce qui rendait impraticable l'exécution d'un modèle plus grand. De plus, on a converti notre modèle au format ONNX pour améliorer sa portabilité et sa compatibilité.

### Vision sur le côté

#### Connexion à la caméra 

La connexion se fait grâce à un protocole RTSP et OpenCV. Ensuite, un script lit continuellement les frames depuis la caméra. 

#### Pre-processing

Les étapes consistent en :

- Limiter à 10 FPS. Cela permet la détection précise des humains en assurant une expérience fluide en temps réel.

- Formater les images capturées afin d'optimiser le traitement par l'IA. On réduit alors la résolution de l'image à moitié.

#### Utilisation de l'IA

On utilise Yolov8nPose comme décrit précedemment. On récupère ensuite le keypoint qui correspond à la cheville de l'humain détecté. On règle le seuil de confiance à 0,6. 

#### Quantification

On initialise une zone dans laquelle nous voulons compter les humains qui s'y trouveront. 

<img src="/assets/filedattente/polyzone.png" alt="polyzone" />

<i>Zone dans laquelle on compte les personnes.</i>

Une variable est initialisé et est incrémenté pour chaque détection de personne dans la zone. Si la cheville est dans la zone et que le seuil de confiance est plus grand que 0,6 alors la personne est comptée.

<img src="/assets/filedattente/compte.gif" alt="compte personne" />

<i>Exemple de la détection d'une personne dans la zone.</i>

#### Envoi par JSON à RequestBin

Une fois le nombre de personne dans la zone comptée, on transforme les données en format JSON puis on envoi les informations à RequestBin avec une requête HTTP. 

### Vision sur le dessus

La version pré-entrainée de YOLOv8Pose ne pouvait pas détecter les personnes efficacement en vision sur le dessus. 

#### Création d'un dataset

Une solution était de créer un dataset avec des images de personnes vues du dessus. 
Pour se faire, on a utilisé Roboflow qui est un outil de gestion de données et d'annotations d'images. On a ainsi annoté approximativement 600 images.

<img src="/assets/filedattente/entrainementimages.png" alt="images dataset" />

<img src="/assets/filedattente/datasetbilan.png" alt="bilan dataset" />

<i>Bilan des annotations du Dataset.</i>

#### Entrainement

L'entrainement du modèle est une étape cruciel pour avoir une bonne performance. L'entrainement s'est fait grâce à Google Colab qui permet d'utiliser ses GPU pour un traitement plus rapide.

On a utilisé le modèle YOLOv5 pour entrainer le modèle avec 100 <i>epochs</i>.

<img src="/assets/filedattente/resultat.png" alt="resultats graphes" />

<i>Résultats de l'entraînement.</i>

L'entraînement ne permet pas d'avoir de bons résultats. La précision devrait être au dessus de 0.8. 
Pour résoudre le problème il serait possible d'augmenter le nombre d'epochs, ou ajouter des images annotées au dataset.

### Problèmes rencontrés 

Pour ce projet il est important de noter plusieurs problèmes rencontrés.

- Manque de temps : ce projet devait être réalisé en 3 semaines, ce qui n'était pas suffisant.

- Puissance du GPU : Mon ordinateur ne disposant pas de GPU, il était compliqué d'entraîner efficacement le modèle. 

### Conclusion et finalité de ce premier projet.

La partie sur la caméra permettant la vue sur le côté fonctionne, mais il serait interessant d'utiliser un modèle de YOLO plus large.

La partie sur la caméra permettant la vue sur le dessus a besoin d'un modèle mieux entrainé, ou d'autres images annotées dans le dataset. 

Un projet futur pourrait être de compter le temps que les personnes passent dans la file d'attente.