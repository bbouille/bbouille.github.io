---
title: Debian wheezy et Raspberry PI (via OS X)
layout: post
permalink: /2013/08/16/debian-wheezy-et-raspberry-pi-quick-start-os-x/
tags: [raspberry]
---
Notes pour le premier démarrage d'une Debian Wheezy sur raspberry pi grandement facilité par un apple script (merci [ivanx][1])

## Pré-requis

### matériel

  * 1 raspberry pi (modèle A ou B)
  * 1 carte SD de 4Go

### logiciel

  * une image ISO* **Raspbian “wheezy”** (actuellement release du 2013-07-26) : [http://www.raspberrypi.org/downloads]()
  * un script d'initialisation de la carte SD, nommé **pi filter** (actuellement en v1.1.1) : [Pi Filler 1.1.1][2]

**note**:  non compatible avec des JVM Oracle, si vous souhaité exécuter une JVM, prenez l'image **Soft-float Debian “wheezy”**.

## Installation

A partir des versions publiées lors de la rédaction du post :

  * Décompresser l'archive : 2013-07-26-wheezy-raspbian.zip
  * Décompresser PiFiller.zip
  * Executer le script **Pi Filler** et suivez les instructions (en anglais).

That's it ! La carte SD est prête à être placée dans le raspberry pi pour un premier démarrage.

 [1]: http://ivanx.com/
 [2]: http://ivanx.com/raspberrypi/files/PiFiller.zip