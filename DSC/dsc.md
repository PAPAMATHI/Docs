---
title: "FONCTIONNEMENT ET UTILISATION DU BANC DSC"
geometry: margin=2cm
output: pdf_document
---

# Informations :
- Auteur : DA SILVA Mathieu
- Révision : 0,1
- Date de création : 21/05/2024

# Révisions :
- 21/05/2024 :
	Ajout d'explication sur le fonctionnement du programme de test.

# Equippement du banc DSC :
Le banc DSC est un ensemble de programmes, serveurs virtuels et de commutateurs réseaux.

## Liste du matériel :
- Serveur ProxMox.
- Routeur pfSense.
- VM almalinux.
- Commutateur outillage.
- Ordinateur outillage 

## Configuration réseaux :
- Masque de sous réseaux : 255.255.255.0.
- Adresse de la passerelle : 192.168.1.1.
- Adresse du réseaux : 192.168.1.1.

## Utilisation du matériel :
- **Serveur ProxMox :** Le serveur proxmox est utiliser pour héberger des machines virtuelles (VM -> Virtual Machine).
Pour toutes informations complémentaires, se referer à **DA SILVA Mathieu** ou au site constructeur via [ce lien](https://www.proxmox.com/en/proxmox-virtual-environment/overview).\
Proxmox est administrable via une **Interface Web**, pour cela, il faut se connecter via un navigateur WEB en **https** à l'**adresse IP** du serveur via le port **8006**.

	Adresse du serveur proxmox : *https://192.168.1.100:8006*
  
- **Routeur pfSense : ** pfSense est un **FireWall** open-source, il permet principalement de créer et ou de manager un ou plusieurs réseaux/sous réseaux.
Dans notre cas, pfSense est installé sur une VM du serveur proxmox.\
la VM dispose de deux interface réseaux :
1. WAN : Adresse du réseaux externe.
2. LAN : Adresse du réseaux interne.\
pfSense est également administrable via un navigateur en vous connectant à l'adresse IP du routeur **192.168.1.1**  

- **VM Almalinux** : Une distribution **Almalinux** est installée sur cette VM. Cependant, elle est exclusivement utilisé en ligne de commande, car elle permet de tester le transfert de fichier entre l'ordinateur outillage et celle-ci.

- **Ordinateur outillage** : Une distribution **Almalinux** avec interface graphique est installée dessus pour pouvoir éxecuter des scipts et manager les différents serveurs.

- **Commutateur Outillage** : Celui-ci, sert à connecter au réseau **LAN** du pfSense les autres appareils (PC outillage, Commutateur client, etc...)

# Procédure de test :
1. Connectez-vous au PC outillage avec les identifiants ci-dessous :\
	**login :** cots\
	**password :** password\

2. Brancher le port n°1 du commutateur client sur le switch outillage.\

3. Ouvrez un terminal puis tapez la ligne de commande suivante :\
	```
	./home/cots/dsc/dsctest.sh
	```

4. Lorsque le programme de test se lance, indiquer le nombre de port présent sur le switch client.


5. Le programme va automatiquement tester via protocole **ICMP** la communication entre l'ordinateur outillage et le routeur.

	5.1 Si le résultat est **NOK**, vérifier que le commutateur client est bien relié au switch outillage.

	5.2 Si le commutateur client est correctement relié au switch outillage, poursuivre le test et connecter le port n°2 du commutateur client au switch outillage.

6. Le programme affiche les résultats du test n°1 :
	- **paquets valides** : x
	- **paquets invalides** : y
	- **TEST OK** ou **TEST NOK**

7. Le programme va automatiquement tester un transfert de fichier volumineux via **scp**

8. Le programme affiche les résultats du test n°2 :
