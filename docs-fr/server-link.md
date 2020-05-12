# Liaison Site-Serveur

## Introduction

Il est nécesaire pour certaines fonctionnalités du site web d'activer
la liaison de votre site web et de votre/vos serveurs Minecraft. Sur
Azuriom, vous pouvez lier votre serveur Minecraft de différentes facons:

* Par Ping - **C'est la solution la plus simple, mais aussi la plus limitée** :
Le ping permet juste de récupérer le nombre de joueurs connectés au serveur
_(ne permet pas d'exécuter des commandes)_.

* Par Rcon - **C'est la solution intermédiaire** :
La liaison rcon permet d'éxecuter des commandes et de récupérer certaines informations cruciales
comme le nombre de joueur ou encore le tps du serveur.

* Par Plugin - **C'est la meilleure solution** :
Le plugin azauth rend la liaison avec le serveur très simple et 
efficace et permet d'avoir accès à toutes les fonctionnalités du site

## Liaison par Ping

Pour pouvoir lier un serveur avec un site sous Azuriom par ping, 
il faut juste ajouter un nouveau serveur avec comme type de liaison "Ping"
et remplir les informations demandées _(le port par défaut de Minecraft est `25565`)_.

## Liaison par Rcon

Pour pouvoir lier un serveur avec un site sous Azuriom par Rcon, il faut:

1. Vous rendre dans le fichier `server.properties` de votre serveur

1. Configurer ce fichier de la façon suivante:
    * Mettre `enable-rcon` en `true`
    * Mettre `rcon.password` avec `votre-mot-de-passe`
    * Mettre `rcon.port` avec `votre-port` _(par défaut `25575`)_
    * Sauvegarder et redémarrer votre serveur
   
1. Vous rendre sur votre site et ajouter un nouveau serveur avec comme type de liaison "Rcon"
et remplir les informations demandées _(le port Rcon par défaut est `25575`)_.

## Liaison par plugin (AzLink)

### Qu'est-ce que AzLink ?

AzLink est un plugin de liaison site-serveur spécialement conçu pour et par Azuriom 
afin de vous permettre de lier un serveur à un site sous Azuriom de façon simple,
rapide et sécurisée.

Actuellement AzLink supporte Bukkit/Spigot, BungeeCord, Sponge et Velocity dans le même plugin.
Une version legacy est disponible pour les serveurs utilisant Bukkit 1.7.10.

### Installation

1. Télécharger AzLink sur [notre site](https://azuriom.com/azlink)

1. Installer AzLink sur le serveur en le mettant dans le dossier `plugins/`
(ou `mods/` pour Sponge) et redémarrer le serveur.

1. Aller sur le site et ajouter un nouveau serveur avec comme type de liaison "AzLink", 
suivre les étapes de liaison et remplir les informations demandées.
