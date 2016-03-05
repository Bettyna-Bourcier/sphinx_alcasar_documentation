============
Installation
============

Introduction
============

Ce document décrit la procédure d’installation du portail ALCASAR. Il est complété par trois autres documents : le document de présentation,
le document d’exploitation et la documentation technique.

Si vous possédez déjà une version d’ALCASAR fonctionnelle et que vous désirez effectuer une mise à jour, reportez-vous à la documentation d’exploitation (chapitre « mise à jour »).

ALCASAR peut être installé sur un ordinateur standard équipé de deux cartes réseau Ethernet. La première est connectée à l’équipement du Fournisseur d’Accès Internet (FAI).
La deuxième est connectée au commutateur utilisé pour desservir le réseau des stations de consultation.

Par défaut, l’adresse IP de cette deuxième carte réseau est : 192.168.182.1/24. Cela permet de disposer d’un plan d’adressage de classe C (254 équipements).
Ce plan d’adressage est modifiable lors de l’installation. Pour tous les équipements situés sur le réseau de consultation, ALCASAR est le serveur DHCP,
le serveur DNS, le serveur de temps et le « routeur par défaut (default gateway) ».
**Ainsi, sur ce réseau, il ne doit y avoir aucun autre routeur ou serveur DHCP** (vérifiez bien vos points d’accès WIFI).

.. image:: images/schema_architecture_globale.PNG

**Exemple d’un plan d’adressage de classe C (254 équipements)**

* adresses IP disponibles : de 192.168.182.2 à 192.168.182.254 (statiques ou dynamiques)
* masque de réseau : 255.255.255.0
* adresses des serveurs DNS et du routeur par défaut (default gateway) : 192.168.182.1 (adresse IP d’ALCASAR)
* suffixe DNS pour les équipements en adressage fixe : « localdomain »


**Exemple d’un plan d’adressage de classe B (65534 équipements)**

* Adresse IP d’ALCASAR : 172.16.0.1/16
* Nombre maximum d’équipements sur le réseau de consultation : 65531
* Paramètre des équipements de consultation :

  * adresses IP disponibles : de 172.16.0.2 à 172.16.255.254 (statiques ou dynamiques)
  * masque de réseau : 255.255.0.0
  * adresses des serveurs DNS et du routeur par défaut (default gateway) : 172.16.0.1 (adresse IP d’ALCASAR)
  * suffixe DNS pour les équipements en adressage fixe : « localdomain »

Bien que cela soit possible, il est déconseillé de définir un réseau de consultation en classe A (ex : 15.0.0.0/8).
En effet, le serveur DHCP interne d’ALCASAR devra alors réserver et gérer plus de 16 millions d’adresses IP.
La gestion d’un tel volume d’adresses est très gourmande en ressource système et mémoire.

Installation
============

L’installation du portail s’effectue en deux étapes.
La première étape est l’installation d’un système Linux minimaliste basé sur Mageia 4.1.
La deuxième étape permet d’installer et de configurer les différentes briques logicielles constituant ALCASAR.

Besoins matériels
-----------------

ALCASAR n’exige qu’un PC bureautique standard possédant 2 cartes réseau et un disque dur d’une capacité de 100Go au minimum afin d’être en mesure de stocker
les fichiers journaux liés à la traçabilité des connexions.
Les architectures 32 bits et 64 bits sont supportées et automatiquement prises en compte.
ALCASAR intègre plusieurs systèmes optionnels de filtrage (protocoles réseau, adresses IP, URL, noms de domaines et antimalware).
Si vous décidez d’activer ces systèmes de filtrage, il est recommandé d’installer au moins 8 GO de mémoire vive afin d’assurer une rapidité de traitement acceptable
(ALCASAR aime la RAM ;-) ).

.. note:: Cas d’une Machine Virtuelle : la taille du disque dur virtuel **ne doit pas être inférieure à 25G**.

Installation du système
-----------------------

La procédure d’installation de ce système est la suivante (durée estimée : 6’) :

* récupérez l’image ISO du DVD de Mageia4.1 double architecture (32 et 64 bits) : fichier « mageia4.1-dual-DVD.iso » (1GB).
  Cette image ISO est disponible sur le site d’ALCASAR ainsi que sur les `sites miroirs Mageia <http://mirrors.mageia.org/>`_.
  Par exemple :

  * http://www.mirrorservice.org/sites/mageia.org/pub/mageia/iso/4.1/
  * http://distrib-coffee.ipsl.jussieu.fr/pub/linux/Mageia/iso/4.1/

* gravez cette image sur un DVD-ROM ou créez une clé USB amorçable1. Vous pouvez aussi utiliser un disque dur externe simulant un périphérique
  amorçable (ex : zalman zm-ve300 ou 400).
* modifiez les paramètres BIOS du PC afin de supprimer l’option « Secure Boot », de régler la date, l’heure et afin de permettre l’amorçage du PC
  à partir d’un DVD-ROM ou d’une clé USB. À la fin de l’installation, modifiez une nouvelle fois les paramètres BIOS pour limiter les possibilités
  d’amorçage du PC au seul disque dur ;
* insérez le DVD-ROM ou la clé USB, redémarrez le PC et suivez les instructions suivantes :

+-------------------------------------------------------+------------------------------------------------------------------------------+--------------------------------------+
|  Messages affichés à l'écran                          |      Commentaires                                                            |      Actions à réaliser              |
+=======================================================+==============================================================================+======================================+
|.. image:: images/mageia_installation_menu.png         || Après démarrage du PC, cette page d’accueil est présentée.                  | Sélectionnez « Install Mageia 4 ».   |
|                                                       ||                                                                             |                                      |
|                                                       ||  * si le mode graphique n’apparaît pas, vous devez configurer le BIOS du PC |                                      |
|                                                       ||  afin d’allouer plus de 2Mo de la mémoire partagée pour la carte graphique. |                                      |
+-------------------------------------------------------+------------------------------------------------------------------------------+--------------------------------------+
|.. image:: images/mageia_installation_choix_langue.png |                                                                              | Séletionnez votre langue.            |
|                                                       |                                                                              |                                      |
+-------------------------------------------------------+------------------------------------------------------------------------------+--------------------------------------+
|.. image:: images/mageia_installation_licence.png      |                                                                              || Acceptez le contrat de licence.     |
|                                                       |                                                                              ||                                     |
|                                                       |                                                                              || Info : ce contrat explique que les  |
|                                                       |                                                                              || logiciels installés sont des        |
|                                                       |                                                                              || logiciels libres.                   |
+-------------------------------------------------------+------------------------------------------------------------------------------+--------------------------------------+
|.. image:: images/mageia_installation_clavier.png      |                                                                              || Sélectionnez votre type de          |
|                                                       |                                                                              || clavier.                            |
+-------------------------------------------------------+------------------------------------------------------------------------------+--------------------------------------+
|.. image:: images/mageia_installation_partition.png    || Le partitionnement du disque dur sera adapté au besoin d’ALCASAR            || Sélectionnez                        |
|                                                       || (cf. étape suivante).                                                       || « Partitionnement de                |
|                                                       ||                                                                             || disque personnalisé ».              |
+-------------------------------------------------------+------------------------------------------------------------------------------+--------------------------------------+
|.. image:: images/mageia_installation_partition_2.png  || Après avoir supprimé toutes les partitions, créez les 5 partitions          || Cliquez sur « Supprimer toutes      |
|                                                       || suivantes :                                                                 || les partitions ».                   |
|                                                       ||                                                                             ||                                     |
|                                                       || - / : 4 Go                                                                  || Cliquez ensuite à l’intérieur de la |
|                                                       || - swap : gardez la taille proposée                                          || zone grise du disque (sda) pour     |
|                                                       || - /tmp : 4 Go                                                               || créer chaque nouvelle partition.    |
|                                                       || - /home : 4 Go                                                              ||                                     |
|                                                       || - /var : le reste du disque dur (**taille supérieure à 10G, même sur une**  || Info : mise à part la partition de  |
|                                                       ||   **machine virtuelle**).                                                   || « swap », tous les Systèmes de      |
|                                                       ||                                                                             || Fichiers (SF) sont du type .        |
|                                                       ||                                                                             || « Journalized FS : ext4 »           |
+-------------------------------------------------------+------------------------------------------------------------------------------+--------------------------------------+
|.. image:: images/mageia_installation_partition_3.jpg  | .. image:: images/mageia_installation_partition_4.jpg                        || Créez la partition racine (/).      |
|                                                       ||                                                                             || Choisissez sa taille (4 Go) ainsi   |
|                                                       || À la fin de cette opération, et en fonction de la taille de                 ||  que son système de fichier (ext4). |
|                                                       || votre disque dur, le partitionnement devrait ressembler à l'image           ||                                     |
|                                                       || ci-dessus.                                                                  || Recommencez cette étape pour        |
|                                                       ||                                                                             || toutes les autres partitions.       |
|                                                       ||                                                                             ||                                     |
|                                                       ||                                                                             || Une fois le partitionnement         |
|                                                       ||                                                                             || effectué, cliquez sur « Terminer ». |
+-------------------------------------------------------+------------------------------------------------------------------------------+--------------------------------------+
|.. image:: images/mageia_installation_media.png        | Pour ALCASAR, l'installation ne nécessaite pas d'autre média.                || Sélectionnez "Aucun" puis           |
|                                                       |                                                                              || cliquer sur "Suivant"               |
+-------------------------------------------------------+------------------------------------------------------------------------------+--------------------------------------+
|.. image:: images/mageia_installation_media.png        | Pour ALCASAR, l'installation ne nécessaite pas d'autre média.                || Laissez le média                    |
|                                                       |                                                                              || « Nonfree release » activé          |
|                                                       |                                                                              || puis cliquez sur « Suivant ».       |
+-------------------------------------------------------+------------------------------------------------------------------------------+--------------------------------------+
|.. image:: images/mageia_installation_paquetage.png    || Sélection des groupes de paquetages à installer :                           || Cliquez sur « Supprimer toutes      |
|                                                       || ALCASAR ne nécessite qu’une installation très minimaliste du système.       || les partitions ».                   |
|                                                       ||                                                                             ||                                     |
|                                                       || - / : 4 Go                                                                  || Cliquez ensuite à l’intérieur de la |
|                                                       || - swap : gardez la taille proposée                                          || zone grise du disque (sda) pour     |
|                                                       || - /tmp : 4 Go                                                               || créer chaque nouvelle partition.    |
|                                                       || - /home : 4 Go                                                              ||                                     |
|                                                       || - /var : le reste du disque dur (**taille supérieure à 10G, même sur une**  || Info : mise à part la partition de  |
|                                                       ||   **machine virtuelle**).                                                   || « swap », tous les Systèmes de      |
|                                                       ||                                                                             || Fichiers (SF) sont du type .        |
|                                                       ||                                                                             || « Journalized FS : ext4 »           |
+-------------------------------------------------------+------------------------------------------------------------------------------+--------------------------------------+
