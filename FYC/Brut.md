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


EXT4 est le système de fichier par défaut pour les distributions Linux basés sur Debian ou Ubuntu. EXT4 est un système par journalisation, c'est à dire qu'il enregistre au sein d'un journal chaque modification sur les métadonnées avant de les réaliser. 

Grâce à l'utilisation des extents qui sont des suites contiguës de blocs physiques présent sur le disque et d'autres mécanismes, EXT4 permet réduire la fragmentation des données au sein du disque. 

Il est parfait pour des cas d'utilisations généraux comme un poste de travail.

XFS est le système de fichier par défaut pour les distributions Linux basés sur RHEL. XFS est aussi un système de fichier par journalisation et utilise des extents comme EXT4. Cependant XFS est optimisé pour la gestion de large volumes de données gourmands en lecture/écriture. XFS vise à répondre au stockage de grandes bases de données ou de fichiers ou de média volumineux.

ZFS est un système de fichiers Open Source dont le développement est devenu propriétaire suite au rachat par Oracle. Il existe une implémentation Open Source nommé OpenZFS qui n'est pas intégré au noyau Linux à cause d'une incompatibilité de licence. ZFS est à la fois un système de fichier et un gestionnaire de volume comme LVM. C'est un système de fichier en Copy on Write, c'est à dire qu'à chaque modification, il écrit les nouvelles données dans un espace distinct sans modifier le fichier d'origine. ZFS mutualise les disques pour créer des pools de stockage qui serviront à créer des volumes. Il intègre aussi plusieurs fonctionnalités comme les checksums, la compression, les snapshots, les RAID-Z qui sont des implémentations RAID optimisé pour ZFS ou encore Scrub qui fait de la correction d'erreur en arrière plan. Tout ceci fait de ZFS un système de fichier très puissant mais aussi très gourmant en ressources. Il est conçu pour le stockage des données critiques ou de la sauvegarde.


BTRFS est un système de fichier pensé pour être un équivalent à ZFS directement intégré au noyau Linux. C'est un système de fichier en Copy on Write, il utilise lui aussi des pools de stockages servant à faire des volumes. il intègre des snapshots, de la compression, des checksums ou encore des implémentations RAID. BTRFS consomme moins de ressource que ZFS mais est bien plus jeune et certaine de ses fonctionnalités sont encore instables. BTRFS est assez flexible et sera parfait pour les systèmes exploitant ses fonctionnalités comme Docker/Podman.


ReFS est un système de fichier exclusif aux solutions Windows et est développé par Microsoft. Il est optimisé pour le stockage de grandes quantités de données critiques. Il intègre des fonctionnalités de checksums, de réparation automatique des données et surtout il sépare les données des métadonnées. ReFS est aussi taillé pour la haute disponibilité et s'adapte très bien aux environnements en cluster. Cependant, il n'est pas encore possible d'utiliser ReFS comme système de fichier de démarrage. adapté pour le données critiques et/ou volumineuses, pour le stockage de machines virtuelles Hyper-V.
