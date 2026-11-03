# Sujet

# V1
Sujet: Comment monter son propre Cloud ? 
Description: Comment les cloud providers de nos jours (AWS, GCP, Azure) font pour mettre à disposition des ressources de calculs, stockage, réseau et sécurité ?


# V2
Sujet: Comment monter son propre Cloud ? du hardware au serverless
Description: Comment une architecture peut-elle se définir selon différents besoins ? Comment les cloud providers de nos jours (AWS, GCP, Azure) font pour mettre à disposition des ressources de calculs, stockage, réseau et sécurité ?


Le but de ce cours est d'aborder les différents concepts réseaux, stockages, hardware et logicielles permettant de mettre en place une solution cloud sans forcément se concentre sur technologie définie.


Vidéo présentation
TP2
Glossaire/Lexique

Etat de l'art
Rapport de veille


EXT4 est le système de fichier par défaut pour les distributions Linux basés sur Debian ou Ubuntu. EXT4 est un système par journalisation, c'est à dire qu'il enregistre au sein d'un journal chaque modification sur les métadonnées avant de les réaliser. EXT4 utilise des extents qui sont des suites contiguës de blocs physique du disque ainsi que d'autres mécanismes pour réduire la fragmentation. Il est parfait pour des cas d'utilisations généraux comme un poste de travail.

XFS est le système de fichier par défaut pour les distributions Linux basés sur RHEL. XFS est aussi un système de fichier par journalisation et utilise des extents comme EXT4. Cependant XFS est optimisé pour la gestion de large volumes de données gourmands en lecture/écriture. XFS vise à répondre au stockage de grandes bases de données ou de fichiers ou de média volumineux.

ZFS est un système de fichiers Open Source dont le développement est devenu propriétaire suite au rachat par Oracle. Il existe une implémentation Open Source nommé OpenZFS qui n'est pas intégré au noyau Linux à cause d'une incompatibilité de licence. ZFS est à la fois un système de fichier et un gestionnaire de volume comme LVM. C'est un système de fichier en Copy on Write, c'est à dire qu'à chaque modification, il écrit les nouvelles données dans un espace distinct sans modifier le fichier d'origine. ZFS mutualise les disques pour créer des pools de stockage qui serviront à créer des volumes. Il intègre aussi plusieurs fonctionnalités comme les checksums, les snapshots, les RAID-Z qui sont des implémentations RAID optimisé pour ZFS ou encore Scrub qui fait de la c