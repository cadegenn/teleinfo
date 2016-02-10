> L'auteur originel est [Charles-Henri Hallard](https://github.com/hallard). Merci à lui.
> Ci dessous le README amendé par mes modifs.

Repository dédié à la Téléinformation
=====================================

Ce dossier à la base était pour le programme téléinfo broadcast et emoncms, mais il est devenu au fur et à mesure un peu le fourre tout de mes projets concernant la téléinformation.
Dorénavant, je crée des repos séparés pour les nouveaux projets 

Dans ce repo vous trouverez :
- Le dongle [Micro Teleinfo](https://github.com/hallard/teleinfo/tree/master/MicroTeleinfo) ainsi que les liens vers la documentation
- Le shield Raspberry [Pi PiTinfo](https://github.com/hallard/teleinfo/tree/master/PiTInfo) ainsi que les liens vers la documentation

Et le nouveau projet Teleinfo Wifi Serveur dans un repo dédié
- [WifInfo](https://github.com/hallard/WifInfo)

Historique de ce repo et programme associé ci-dessous :

Téléinfo, mySQL et Emoncms
====================================
Ce programme permet de récupérer les trames téléinfo reçue par la liaison série d'un ordinateur (ou autre périphérique). 

Il est aussi capable d'envoyer les données de téléinfo vers une base mySQL ou un serveur Emoncms
 
Ce programme fonctionne uniquement sous linux (testé sur Debian Lenny et wheezy) mais çà ne doit pas être sorcier de l'adapter pour Windows.
 
Fidel aux préceptes UNIX, le programme ne fait qu'une seul chose : écouter sur un port série et retranscrire une trame téléinfo.

On peut le compiler avec un support MySQL et/ou EmonCMS (non testé).

Le fonctionnement est le suivant :
  - écouter sur le port série
  - reconstituer une trame entière et valide téléinfo
  - si compilé et exécuté avec le support MySQL, injecter les données dans une base
  - si compilé et exécuté avec le support EmonCMS, envoyer les données à un serveur EmonCMS
  - quitter

Contrairement au programme original, cette version ne peut traiter qu'une seul trame par exécution. 

Suivez les nouveautés et autres projets sur le [blog][4] de Charles-Henri Hallard.

Installation et configuration
==============================

Tout est documenté sur la page dédié [teleinfo][5] de son blog. Mais je vais quand même détailler un peu ici.

Éditer le fichier ``Makefile`` :
  - pour compiler le support MySQL, décommenter les 2 lignes :
``` .sh
#CFLAGSSQL=-DUSE_MYSQL
#LIBSSQL=-lmysqlclient
```
  - pour compiler le support EmonCMS, décommenter les 2 lignes :
``` .sh
#CFLAGSEMONCMS=-DUSE_EMONCMS 
#LIBSEMONCMS=-lcurl 
```
Lancer la commande ``make``, puis ``make install``

Le binaire et le fichier de configuration seront installés par défaut dans */opt/teleinfo/*

# Utilisation

``` shell
/opt/teleinfo/teleinfo -v --tty /dev/ttyAMA0
```

## Support MySQL

Lancer ce code dans MySQL :
``` sql

CREATE DATABASE IF NOT EXISTS `EDF`;

USE `EDF`;

--
-- Structure de la table `tbTeleinfo`
--

CREATE TABLE IF NOT EXISTS `tbTeleinfo` (
  `DATE` datetime DEFAULT NULL,
  `ADCO` varchar(12) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `OPTARIF` varchar(4) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `ISOUSC` varchar(2) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `BASE` varchar(9) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `HCHC` varchar(9) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `HCHP` varchar(49) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `EJPHN` varchar(9) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `EJPHPM` varchar(9) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `BBRHCJB` varchar(9) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `BBRHPJB` varchar(9) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `BBRHCJW` varchar(9) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `BBRHPJW` varchar(9) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `BBRHCJR` varchar(9) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `BBRHPJR` varchar(9) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `PEJP` varchar(2) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `PTEC` varchar(4) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `DEMAIN` varchar(4) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `IINST` varchar(3) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `ADPS` varchar(4) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `IMAX` varchar(4) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `IINST1` varchar(3) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `IINST2` varchar(3) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `IINST3` varchar(3) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `IMAX1` varchar(3) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `IMAX2` varchar(3) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `IMAX3` varchar(3) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `PMAX` varchar(5) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `PAPP` varchar(5) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `HHPHC` varchar(5) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `MOTDETAT` varchar(6) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  `PPOT` varchar(2) CHARACTER SET latin1 COLLATE latin1_general_ci DEFAULT NULL,
  KEY `SEARCH_INDEX` (`ADCO`,`DATE`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8 COLLATE=utf8_bin;
```

Lancer teleinfo :
``` .sh
/opt/teleinfo/teleinfo -q -s localhost -b EDF -t tbTeleinfo -u username -w password --tty /dev/ttyAMA0
```


License
=======

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Licence Creative Commons" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">MicroTeleinfo</span> de <span xmlns:cc="http://creativecommons.org/ns#" property="cc:attributionName">Charles-Henri Hallard</span> est mis à disposition selon les termes de la <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">licence Creative Commons Attribution - Pas d’Utilisation Commerciale - Partage dans les Mêmes Conditions 4.0 International</a>.


[4]: http://hallard.me
[5]: http://hallard.me/teleinfo-emoncms/
