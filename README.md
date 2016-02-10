> L'auteur originel est [Hallard](https://github.com/hallard). Merci à lui.
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

License
=======

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Licence Creative Commons" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">MicroTeleinfo</span> de <span xmlns:cc="http://creativecommons.org/ns#" property="cc:attributionName">Charles-Henri Hallard</span> est mis à disposition selon les termes de la <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">licence Creative Commons Attribution - Pas d’Utilisation Commerciale - Partage dans les Mêmes Conditions 4.0 International</a>.


[4]: http://hallard.me
[5]: http://hallard.me/teleinfo-emoncms/
