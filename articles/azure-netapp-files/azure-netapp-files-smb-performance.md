---
title: Bonnes pratiques relatives aux performances SMB pour Azure NetApp Files | Microsoft Docs
description: Aide à comprendre les performances SMB et les bonnes pratiques pour Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/10/2021
ms.author: b-juche
ms.openlocfilehash: 6c29c1804e5587a2a1a3bae3e2f566a5938350cf
ms.sourcegitcommit: 190658142b592db528c631a672fdde4692872fd8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/11/2021
ms.locfileid: "112004778"
---
# <a name="smb-performance-best-practices-for-azure-netapp-files"></a>Bonnes pratiques relatives aux performances SMB pour Azure NetApp Files

Cet article aide à comprendre les performances SMB et les bonnes pratiques concernant Azure NetApp Files.

## <a name="smb-multichannel"></a>SMB Multichannel

SMB Multichannel est activé par défaut dans les partages SMB. La fonctionnalité était activée pour tous les partages SMB antérieurs aux volumes SMB existants et elle sera également activée pour tous les volumes au moment de leur création. 

Toute connexion SMB établie avant l’activation de la fonctionnalité Multichannel SMB doit être réinitialisée pour tirer parti de celle-ci. Pour effectuer la réinitialisation, vous pouvez déconnecter et reconnecter le partage SMB.

Windows prend en charge SMB Multichannel depuis Windows 2012 pour optimiser les performances.  Pour plus d’informations, consultez [déployer SMB Multichannel](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3Dws.11)) et [Notions de base de SMB Multichannel](/archive/blogs/josebda/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0). 

### <a name="benefits-of-smb-multichannel"></a>Avantages de SMB Multichannel

La fonctionnalité SMB Multichannel permet à un client SMB3 d’établir un pool de connexions sur une ou plusieurs cartes d’interface réseau et de les utiliser pour envoyer des demandes pour une session SMB unique. À l’inverse, du fait de leur conception, SMB1 et SMB2 obligent le client à établir une connexion et à envoyer sur celle-ci tout le trafic SMB pour une session donnée. Cette connexion unique limite les performances globales du protocole qui peuvent être obtenues à partir d’un seul client.

### <a name="performance-for-smb-multichannel"></a>Performances de SMB Multichannel

Les tests et les graphiques suivants illustrent la puissance de SMB Multichannel sur les charges de travail mono-instance.

#### <a name="random-io"></a>E/S aléatoires  

Avec SMB Multichannel désactivé sur le client, des tests de lecture et d’écriture de 4 Kio purs ont été effectués à l’aide de l’outil FIO et d’une plage de travail 40 Gio.  Le partage SMB a été détaché entre chaque test, avec augmentation du nombre de connexions clientes SMB en fonction des paramètres d’interface réseau RSS : `1`,`4`,`8`,`16`, `set-SmbClientConfiguration -ConnectionCountPerRSSNetworkInterface <count>`. Les tests montrent que le paramètre par défaut de `4` est suffisant pour les charges de travail gourmandes en E/S. L’incrémentation à `8` et `16` a eu un effet négligeable. 

D’après la commande `netstat -na | findstr 445`, des connexions supplémentaires ont été établies avec des incréments de `1` à `4` à `8` et à `16`.  Quatre cœurs de processeur ont été entièrement utilisés pour SMB lors de chaque test, comme le confirme la statistique Perfmon `Per Processor Network Activity Cycles` (non incluse dans cet article).

![Graphique affichant une comparaison d’E/S aléatoires de SMB Multichannel.](../media/azure-netapp-files/azure-netapp-files-random-io-tests.png)

La machine virtuelle Azure n’affecte pas les limites d’E/S de stockage de SMB (ou NFS).  Comme indiqué dans le graphique suivant, le type d’instance D32ds a un taux limité de 308 000 pour les IOPS de stockage mises en cache et de 51 200 pour les IOPS de stockage non mises en cache.  Toutefois, le graphique ci-dessus montre beaucoup plus d’E/S sur SMB.

![Graphique affichant un test de comparaison d’E/S aléatoires.](../media/azure-netapp-files/azure-netapp-files-random-io-tests-list.png)

#### <a name="sequential-io"></a>E/S séquentielles 

Des tests similaires aux tests d’E/S aléatoires décrits ci-dessus ont été réalisés avec des E/S séquentielles de 64 Kio. Bien que les augmentations du nombre de connexions clientes par interface réseau RSS au-delà de 4 n’aient eu aucun effet notable sur les E/S aléatoires, il en va différemment pour les E/S séquentielles. Comme le montre le graphique suivant, chaque augmentation est associée à une augmentation correspondante du débit de lecture. Le débit d’écriture est resté le même en raison des restrictions de bande passante du réseau imposées par Azure pour chaque type/taille d’instance. 

![Graphique montrant la comparaison du test de débit.](../media/azure-netapp-files/azure-netapp-files-sequential-io-tests.png)

Azure impose des limites de débit réseau sur chaque type/taille de machine virtuelle. La limite du débit est imposée uniquement au trafic sortant. Le nombre de cartes réseau présentes sur une machine virtuelle n’a aucun impact sur la quantité totale de bande passante disponible pour la machine.  Par exemple, le type d’instance D32ds a une limite réseau imposée de 16,000 Mbits/s (2 000 Mio/s).  Comme le montre le graphique séquentiel ci-dessus, la limite affecte le trafic sortant (écritures), mais pas les lectures multicanal.

![Graphique affichant un test de comparaison d’E/S séquentielles.](../media/azure-netapp-files/azure-netapp-files-sequential-io-tests-list.png)

## <a name="smb-signing"></a>SMB Signing

Le protocole SMB fournit la base pour le partage de fichiers et d’imprimantes ainsi que pour d’autres opérations réseau telles que l’administration à distance de Windows. Pour empêcher des attaques de l’intercepteur, susceptibles de modifier les paquets SMB en transit, le protocole SMB prend en charge la signature numérique des paquets SMB. 

La signature SMB est prise en charge pour toutes les versions de protocole SMB prises en charge par Azure NetApp Files. 

### <a name="performance-impact-of-smb-signing"></a>Impact de SMB Signing sur les performances  

La signature SMB a un effet nuisible sur les performances SMB. Parmi d’autres causes potentielles de la dégradation des performances, la signature numérique de chaque paquet consomme des ressources processeur côté client supplémentaires, comme le montre la sortie Perfmon ci-dessous. Dans ce cas, le cœur 0 semble responsable de SMB, y compris de la signature SMB.  Une comparaison avec les quantités de débit de lecture séquentielle non-multicanal dans la section précédente montre que la signature SMB réduit le débit global de 875 Mio/s à environ 250 Mio/s. 

![Graphique montrant l’impact sur les performances de la signature SMB.](../media/azure-netapp-files/azure-netapp-files-smb-signing-performance.png)


## <a name="performance-for-a-single-instance-with-a-1-tb-dataset"></a>Performances d’une instance avec un jeu de données de 1 To

Pour fournir un aperçu plus détaillé des charges de travail avec des mixages en lecture/écriture, les deux graphiques suivants montrent les performances d’un volume unique de cloud de niveau de service Ultra de 50 To avec un jeu de données de 1 To et avec SMB Multichannel de 4. Un IODepth optimal de 16 a été utilisé, et des paramètres d’E/S flexibles ont été utilisés pour garantir l’utilisation complète de la bande passante réseau (`numjobs=16`).

Le graphique suivant montre les résultats des E/S aléatoires 4k, avec une seule instance de machine virtuelle et une combinaison de lecture/écriture à des intervalles de 10 % :

![Graphique illustrant le test d’E/S aléatoires Windows 2019 standard _D32ds_v4 4 K.](../media/azure-netapp-files/smb-performance-standard-4k-random-io.png)

Le graphique suivant montre les résultats des E/S séquentielles :

![Graphique illustrant le débit séquentiel Windows 2019 standard _D32ds_v4 4 K.](../media/azure-netapp-files/smb-performance-standard-64k-throughput.png)

## <a name="performance-when-scaling-out-using-5-vms-with-a-1-tb-dataset"></a>Performances lors d’un scale-out avec 5 machines virtuelles et un jeu de données de 1 To

Ces tests avec 5 machines virtuelles utilisent le même environnement de test que l’ordinateur virtuel unique, chaque processus écrivant dans son propre fichier.

Le graphique suivant montre les résultats des E/S aléatoires :

![Graphique illustrant le test d’E/S aléatoires d’instances Windows 2019 standard _D32ds_v4 4 K.](../media/azure-netapp-files/smb-performance-standard-4k-random-io-5-instances.png)

Le graphique suivant montre les résultats des E/S séquentielles :

![Graphique illustrant le débit séquentiel d’instances Windows 2019 standard _D32ds_v4 4 K.](../media/azure-netapp-files/smb-performance-standard-64k-throughput-5-instances.png)

## <a name="how-to-monitor-hyper-v-ethernet-adapters"></a>Comment superviser les adaptateurs Ethernet Hyper-V  

Une stratégie utilisée pour le test avec FIO consiste à définir `numjobs=16`. Cela permet de diviser chaque tâche en 16 instances spécifiques pour optimiser la carte réseau Microsoft Hyper-V.

Vous pouvez vérifier l’activité de chacun des adaptateurs dans l’Analyseur des performances Windows en sélectionnant **Analyseur de performances > Ajouter des compteurs > Interface réseau > Carte réseau Microsoft Hyper-V**.

![Capture d’écran montrant l’interface Ajouter un compteur dans l’Analyseur de performances.](../media/azure-netapp-files/smb-performance-performance-monitor-add-counter.png)

Une fois que le trafic de données est en cours d’exécution dans vos volumes, vous pouvez surveiller vos adaptateurs dans l’Analyseur de performances Windows. Si vous n’utilisez pas l’ensemble de ces 16 adaptateurs virtuels, il se peut que vous ne profitiez pas pleinement de la capacité de la bande passante réseau.

![Capture d’écran montrant la sortie de l’Analyseur de performances.](../media/azure-netapp-files/smb-performance-performance-monitor-output.png)

## <a name="smb-encryption"></a>Chiffrement SMB

Cette section permet de mieux comprendre le chiffrement SMB (SMB 3.0 et SMB 3.1.1) 

La fonctionnalité [Chiffrement SMB](/windows-server/storage/file-server/smb-security) permet un chiffrement de bout en bout des données SMB et protège les données contre les tentatives d’écoute clandestine sur des réseaux non approuvés. Le chiffrement SMB est pris en charge sur SMB 3.0 et versions ultérieures. 

Lors de l’envoi d’une demande au stockage, le client chiffre la demande, que le stockage déchiffre ensuite. Les réponses sont chiffrées de façon similaire par le serveur et déchiffrées par le client.

Windows 10, Windows 2012 et les versions ultérieures prennent en charge le chiffrement SMB.

### <a name="smb-encryption-and-azure-netapp-files"></a>Chiffrement SMB et Azure NetApp Files

Le chiffrement SMB est activé au niveau du partage pour Azure NetApp Files. SMB 3.0 utilise l’algorithme AES-CCM, tandis que SMB 3.1.1 utilise l’algorithme AES-GCM.

Le chiffrement SMB n’est pas obligatoire. Par conséquent, il est activé uniquement pour un partage donné si l’utilisateur demande qu’Azure NetApp Files l’active. Les partages Azure NetApp Files ne sont jamais exposés à Internet. Ils sont accessibles uniquement à partir d’un réseau virtuel donné, via VPN ou Express Route, de sorte que les partages Azure NetApp Files sont sécurisés par nature. Le choix d’activer le chiffrement SMB dépend entièrement de l’utilisateur. Tenez compte de la baisse des performances prévue avant d’activer cette fonctionnalité.

### <a name="impact-of-smb-encryption-on-client-workloads"></a><a name="smb_encryption_impact"></a>Impact du chiffrement SMB sur les charges de travail client

Bien que le chiffrement SMB ait un impact sur le client (surcharge du processeur pour le chiffrement et le déchiffrement des messages) et le stockage (réduction du débit), le tableau suivant met en évidence l’impact sur le stockage uniquement. Vous devez tester l’incidence des performances du chiffrement sur vos applications avant de déployer des charges de travail en production.

|     Profil d’E/S       |     Impact        |
|-  |-  |
|     Charges de travail d’écriture et de lecture      |     10 % à 15 %        |
|     Utilisation intensive des métadonnées        |     5 %    |

## <a name="accelerated-networking"></a>Mise en réseau accélérée 

Pour des performances optimales, il est recommandé de configurer l’[accélération réseau](../virtual-network/create-vm-accelerated-networking-powershell.md) sur vos machines virtuelles dès que vous le pouvez. Gardez à l’esprit les points suivants :  

* Le portail Azure active l’accélération réseau par défaut pour les machines virtuelles prenant en charge cette fonctionnalité.  Toutefois, il est possible que ce ne soit pas le cas d’autres méthodes de déploiement telles qu’Ansible et les outils de configuration similaires.  L’échec de l’activation de l’accélération réseau peut entraver les performances d’une machine.  
* Si l’accélération réseau n’est pas activée sur l’interface réseau d’une machine virtuelle en raison de l’absence de prise en charge d’un type ou d’une taille d’instance, elle reste désactivée avec les types d’instances plus grands. Dans ces cas, vous avez besoin d’une intervention manuelle.

## <a name="rss"></a>RSS 

Azure NetApp Files prend en charge la mise à l’échelle côté réception (RSS).

Quand SMB Multichannel est activé, un client SMB3 établit plusieurs connexions TCP au serveur SMB Azure NetApp Files sur une seule carte réseau qui prend en charge RSS. 

Pour voir si vos cartes réseau de machines virtuelles Azure prennent en charge RSS, exécutez la commande `Get-SmbClientNetworkInterface` suivante et vérifiez le champ `RSS Capable` : 

![Capture d’écran montrant une sortie RSS pour une machine virtuelle Azure.](../media/azure-netapp-files/azure-netapp-files-formance-rss-support.png)


## <a name="multiple-nics-on-smb-clients"></a>Présence de plusieurs cartes réseau sur un client SMB

Vous ne devez pas configurer plusieurs cartes réseau sur votre client SMB. Le client SMB s’adapte au nombre de cartes réseau retourné par le serveur SMB.  Chaque volume de stockage est accessible à partir d’un seul point de terminaison de stockage.  Cela signifie qu’une seule carte réseau est utilisée pour une relation SMB donnée.  

Comme le montre la sortie de la commande `Get-SmbClientNetworkInterace` ci-dessous, la machine virtuelle a deux interfaces réseau : 15 et 12.  Comme indiqué sous la commande `Get-SmbMultichannelConnection` suivante, même s’il existe deux cartes réseau qui prennent en charge RSS, seule l’interface 12 est utilisée en relation avec le partage SMB ; l’interface 15 n’est pas utilisée.

![Capture montrant la sortie des cartes réseau prenant en charge RSS.](../media/azure-netapp-files/azure-netapp-files-rss-capable-nics.png)

## <a name="next-steps"></a>Étapes suivantes  

- [Questions fréquentes (FAQ) sur Azure NetApp Files](azure-netapp-files-faqs.md)
- Consultez [Azure NetApp Files: Managed Enterprise File Shares for SMB Workloads](https://cloud.netapp.com/hubfs/Resources/ANF%20SMB%20Quickstart%20doc%20-%2027-Aug-2019.pdf?__hstc=177456119.bb186880ac5cfbb6108d962fcef99615.1550595766408.1573471687088.1573477411104.328&__hssc=177456119.1.1573486285424&__hsfp=1115680788&hsCtaTracking=cd03aeb4-7f3a-4458-8680-1ddeae3f045e%7C5d5c041f-29b4-44c3-9096-b46a0a15b9b1) pour plus d’informations sur l’utilisation de partages de fichiers SMB avec Azure NetApp Files.
