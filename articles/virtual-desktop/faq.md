---
title: FAQ Azure Virtual Desktop - Azure
description: Questions fréquentes et meilleures pratiques concernant Azure Virtual Desktop.
author: Heidilohr
ms.topic: conceptual
ms.date: 03/09/2021
ms.author: helohr
ms.custom: devx-track-azurepowershell
manager: femila
ms.openlocfilehash: 604f7ac3d06ceb9ea981deb6aba4e9f208f06e20
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/09/2021
ms.locfileid: "111757712"
---
# <a name="azure-virtual-desktop-faq"></a>Questions fréquentes (FAQ) sur Azure Virtual Desktop

Cet article répond à des questions fréquentes et décrit les meilleures pratiques concernant Azure Virtual Desktop.

## <a name="what-are-the-minimum-admin-permissions-i-need-to-manage-objects"></a>Quelles sont les autorisations d’administrateur minimales dont j’ai besoin pour gérer les objets ?

Si vous souhaitez créer des pools d’hôtes et d’autres objets, vous devez disposer du rôle Contributeur sur l’abonnement ou le groupe de ressources que vous utilisez.

Vous devez disposer du rôle Administrateur de l’accès utilisateur sur un groupe d’applications pour publier des groupes d’applications auprès d’utilisateurs ou de groupes d’utilisateurs.

Pour limiter un administrateur à la seule gestion de sessions d’utilisateurs, comme l’envoi de messages aux utilisateurs, la déconnexion des utilisateurs, etc., vous pouvez créer des rôles personnalisés. Par exemple :

```powershell
"actions": [
"Microsoft.Resources/deployments/operations/read",
"Microsoft.Resources/tags/read",
"Microsoft.Authorization/roleAssignments/read",
"Microsoft.DesktopVirtualization/hostpools/sessionhosts/usersessions/*",
"Microsoft.DesktopVirtualization/hostpools/sessionhosts/read",
"Microsoft.DesktopVirtualization/hostpools/sessionhosts/write"
],
"notActions": [],
"dataActions": [],
"notDataActions": []
}
```

## <a name="does-azure-virtual-desktop-support-split-azure-active-directory-models"></a>Azure Virtual Desktop prend-il en charge les modèles Azure Active Directory fractionnés ?

Lorsqu’un utilisateur est affecté à un groupe d’applications, le service effectue une simple attribution de rôle Azure. Par conséquent, le compte Azure Active Directory (AD) de l’utilisateur et l’instance Azure AD du groupe d’applications doivent se trouver au même emplacement. Tous les objets de service, tels que les pools d’hôtes, les groupes d’applications et les espaces de travail, doivent également se trouver dans la même instance Azure AD que le compte de l’utilisateur.

Vous pouvez créer des machines virtuelles dans un autre compte Azure AD tant que vous synchronisez le service Active Directory avec le compte Azure AD de l’utilisateur dans le même réseau virtuel (VNET).

## <a name="what-are-location-restrictions"></a>Que sont les restrictions d’emplacement ?

Toutes les ressources de service sont associées à un emplacement. L’emplacement d’un pool d’hôtes détermine la géographie dans laquelle les métadonnées de service du pool d’hôtes sont stockées. Un groupe d’applications ne peut pas exister sans pool d’hôtes. Si vous ajoutez des applications à un groupe d’applications RemoteApp, vous aurez également besoin d’un hôte de session pour déterminer les applications du menu Démarrer. Pour toute action de groupe d’applications, vous aurez également besoin d’un accès aux données connexes sur le pool d’hôtes. Pour vous assurer que les données ne sont pas transférées entre plusieurs emplacements, l’emplacement du groupe d’applications doit être le même que celui du pool d’hôtes.

Les espaces de travail doivent également se trouver au même emplacement que leurs groupes d’applications. Chaque fois que l’espace de travail est mis à jour, le groupe d’applications correspondant est mis à jour en même temps. Comme pour les groupes d’applications, le service requiert que tous les espaces de travail soient associés à des groupes d’applications créés au même emplacement.

## <a name="how-do-you-expand-an-objects-properties-in-powershell"></a>Comment développer les propriétés d’un objet dans PowerShell ?

Lorsque vous exécutez une cmdlet PowerShell, vous voyez uniquement le nom et l’emplacement de la ressource.

Par exemple :

```powershell
Get-AzWvdHostPool -Name 0224hp -ResourceGroupName 0224rg

Location Name   Type
-------- ----   ----
westus   0224hp Microsoft.DesktopVirtualization/hostpools
```

Pour afficher toutes les propriétés d’une ressource, ajoutez `format-list` ou `fl` à la fin de la cmdlet.

Par exemple :

```powershell
Get-AzWvdHostPool -Name 0224hp -ResourceGroupName 0224rg |fl
```

Pour afficher des propriétés spécifiques, ajoutez les noms de propriétés spécifiques après `format-list` ou `fl`.

Par exemple :

```powershell
Get-AzWvdHostPool -Name demohp -ResourceGroupName 0414rg |fl CustomRdpProperty

CustomRdpProperty : audiocapturemode:i:0;audiomode:i:0;drivestoredirect:s:;redirectclipboard:i:1;redirectcomports:i:0;redirectprinters:i:1;redirectsmartcards:i:1;screen modeid:i:2;
```

## <a name="does-azure-virtual-desktop-support-guest-users"></a>Azure Virtual Desktop prend-il en charge les utilisateurs invités ?

Azure Virtual Desktop ne prend pas en charge les comptes d’utilisateur invité Azure AD. Par exemple, imaginons qu’un groupe d’utilisateurs invités ait des licences Microsoft 365 E3 par utilisateur, Windows E3 par utilisateur ou WIN VDA dans sa propre entreprise, mais que les utilisateurs invités appartiennent au compte Azure AD d’une autre entreprise. L’autre entreprise gérerait les objets utilisateur des utilisateurs invités dans les comptes Azure AD et Active Directory comme des comptes locaux.

Vous ne pouvez pas utiliser vos propres licences au profit d’un tiers. En outre, Azure Virtual Desktop ne prend pas actuellement en charge Microsoft Account (MSA).

## <a name="why-dont-i-see-the-client-ip-address-in-the-wvdconnections-table"></a>Pourquoi ne puis-je pas afficher l’adresse IP du client dans la table WVDConnections ?

Actuellement, nous ne disposons pas d’un moyen fiable de collecter les adresses IP du client web, c’est pourquoi nous n’incluons pas cette valeur dans la table.

## <a name="how-does-azure-virtual-desktop-handle-backups"></a>Comment Azure Virtual Desktop gère-t-il les sauvegardes ?

Il existe plusieurs options dans Azure pour gérer les sauvegardes. Vous pouvez utiliser la sauvegarde Azure, Site Recovery et des instantanés.

## <a name="does-azure-virtual-desktop-support-third-party-collaboration-apps"></a>Azure Virtual Desktop prend-il en charge les applications de collaboration tierces ?

Azure Virtual Desktop est actuellement optimisé pour Teams. Microsoft ne prend actuellement pas en charge les applications de collaboration tierces telles que Zoom. Les organisations tierces sont chargées de fournir des directives de compatibilité à leurs clients. Azure Virtual Desktop ne prend pas non plus en charge Skype Entreprise.

## <a name="can-i-change-from-pooled-to-personal-host-pools"></a>Puis-je passer d’un pool d’hôtes groupé à un pool d’hôtes personnel ?

Une fois que vous avez créé un pool d’hôtes, vous ne pouvez plus en modifier le type. Toutefois, vous pouvez déplacer toutes les machines virtuelles que vous inscrivez sur un pool d’hôtes vers un autre type de pool d’hôtes.

## <a name="whats-the-largest-profile-size-fslogix-can-handle"></a>Quelle est la plus grande taille de profil que FSLogix peut gérer ?

Les limitations ou quotas dans FSLogix dépendent de la structure de stockage utilisée pour stocker les fichiers VHD(X) de profil utilisateur.

Le tableau suivant donne un exemple du nombre d’IOPS dont un profil FSLogix a besoin pour prendre en charge chaque utilisateur. Les besoins peuvent varier considérablement en fonction de l’utilisateur, des applications et de l’activité de chaque profil.

| Ressource | Condition requise |
|---|---|
| IOPS d’état stable | 10 |
| IOPS de connexion/déconnexion | 50 |

L’exemple de ce tableau est celui d’un utilisateur unique, mais il peut être utilisé pour estimer les besoins relatifs au nombre total d’utilisateurs dans votre environnement. Par exemple, vous avez besoin d’environ 1 000 IOPS pour 100 utilisateurs et environ 5 000 IOPS lors de la connexion et de la déconnexion.

## <a name="is-there-a-scale-limit-for-host-pools-created-in-the-azure-portal"></a>Existe-t-il une limite d’échelle pour les pools d’hôtes créés dans le portail Azure ?

Ces facteurs peuvent influer sur la limite d’échelle des pools d’hôtes :

- Le modèle Azure est limité à 800 objets. Pour en savoir plus, consultez [Abonnement Azure et les limites, quotas et contraintes des services](../azure-resource-manager/management/azure-subscription-service-limits.md#template-limits). Chaque machine virtuelle crée également environ 6 objets, ce qui signifie que vous pouvez créer environ 132 machines virtuelles chaque fois que vous exécutez le modèle.

- Il existe des restrictions sur le nombre de cœurs que vous pouvez créer par région et par abonnement. Par exemple, si vous avez un abonnement Accord Entreprise, vous pouvez créer 350 cœurs. Pour déterminer le nombre de machines virtuelles que vous pouvez créer chaque fois que vous exécutez le modèle, vous devez diviser 350 par le nombre par défaut de cœurs par machine virtuelle ou par votre propre limite de cœurs. Pour plus d’informations, consultez [Limites de machines virtuelles – Azure Resource Manager](../azure-resource-manager/management/azure-subscription-service-limits.md#virtual-machines-limits---azure-resource-manager).

- Le nom du préfixe de machine virtuelle et le nombre de machines virtuelles est inférieur à 15 caractères. Pour plus d’informations, consultez [Règles de nommage et restrictions pour les ressources Azure](../azure-resource-manager/management/resource-name-rules.md#microsoftcompute).

## <a name="can-i-manage-azure-virtual-desktop-environments-with-azure-lighthouse"></a>Puis-je gérer des environnements Azure Virtual Desktop à l’aide d’Azure Lighthouse ?

Azure Lighthouse ne prend pas entièrement en charge la gestion d’environnements Azure Virtual Desktop. Étant donné qu’Azure Lighthouse ne prend pas actuellement en charge la gestion des utilisateurs sur plusieurs locataires Azure AD, les clients d’Azure Lighthouse doivent toujours se connecter au compte Azure AD que les clients utilisent pour gérer les utilisateurs.

Vous ne pouvez pas non plus utiliser les abonnements de bac à sable de fournisseur de solutions Cloud avec le service Azure Virtual Desktop. Pour plus d’informations, consultez [Compte de bac à sable d’intégration](/partner-center/develop/set-up-api-access-in-partner-center#integration-sandbox-account).

Enfin, si vous avez activé le fournisseur de ressources à partir du compte propriétaire du fournisseur de solutions Cloud, les comptes client du fournisseur de services Cloud ne peuvent pas modifier le fournisseur de ressources.

## <a name="how-often-should-i-turn-my-vms-on-to-prevent-registration-issues"></a>À quelle fréquence dois-je activer mes machines virtuelles pour éviter des problèmes d’inscription ?

Une fois que vous avez inscrit une machine virtuelle dans un pool d’hôtes au sein du service Azure Virtual Desktop, l’agent actualise régulièrement le jeton de la machine virtuelle chaque fois que celle-ci est active. Le certificat du jeton d’inscription est valide pendant 90 jours. En raison de cette limite de 90 jours, nous recommandons de mettre en ligne les machines virtuelles pendant 20 minutes tous les 90 jours afin que les machines puissent actualiser les jetons, mettre à jour l’agent et les composants de la pile côte à côte. La désactivation de votre machine virtuelle dans ce délai limitera l’expiration ou la non-validité de son jeton d’inscription. Si vous avez démarré votre machine virtuelle après 90 jours et que vous rencontrez des problèmes d’inscription, suivez les instructions du [Guide de résolution des problèmes de l’agent Azure Virtual Desktop](troubleshoot-agent.md#your-issue-isnt-listed-here-or-wasnt-resolved) pour supprimer la machine virtuelle du pool d’hôtes, réinstaller l’agent, puis réinscrire la machine virtuelle dans le pool.

## <a name="can-i-set-availability-options-when-creating-host-pools"></a>Puis-je définir des options de disponibilité lors de la création de pools d’hôtes ?

Oui. Les pools d’hôtes Azure Virtual Desktop ont une option permettant de sélectionner un groupe à haute disponibilité ou des zones de disponibilité lorsque vous créez une machine virtuelle. Ces options de disponibilité sont les mêmes que celles utilisées par Azure Compute. Si vous sélectionnez une zone pour la machine virtuelle que vous créez dans un pool d’hôtes, le paramètre s’applique automatiquement à toutes les machines virtuelles que vous créez dans cette zone. Si vous préférez répartir vos machines virtuelles de pool d’hôtes dans plusieurs zones, vous devez suivre les instructions fournies dans [Ajouter des machines virtuelles avec le portail Azure](expand-existing-host-pool.md#add-virtual-machines-with-the-azure-portal) pour sélectionner manuellement une nouvelle zone pour chaque nouvelle machine virtuelle que vous créez.

## <a name="which-availability-option-is-best-for-me"></a>Quelle est l’option de disponibilité la mieux adaptée à mes besoins ?

L’option de disponibilité que vous devez utiliser pour vos machines virtuelles dépend de l’emplacement de votre image et de ses champs de disque managé. Le tableau suivant explique la relation de chaque paramètre avec ces variables pour vous aider à déterminer l’option la mieux adaptée à votre déploiement. 

| Option de disponibilité | Emplacement de l’image | Bouton d’option (case d’option) Utiliser un disque managé |
|---|---|---|
| Aucun | Galerie | Désactivé avec « Oui » comme valeur par défaut |
| Aucun | Stockage d'objets blob | Activé avec « Non » comme valeur par défaut |
| Zone de disponibilité | Galerie (option de stockage d’objets blob désactivée) | Désactivé avec « Oui » comme valeur par défaut |
| Groupe à haute disponibilité avec SKU géré (disque managé) | Galerie | Désactivé avec « Oui » comme valeur par défaut |
| Groupe à haute disponibilité avec SKU géré (disque managé) | Stockage d'objets blob | Activé avec « Non » comme valeur par défaut |
| Groupe à haute disponibilité avec SKU géré (disque managé) | Stockage d’objets blob (option Galerie désactivée) | Activé avec « Non » comme valeur par défaut |
| Groupe à haute disponibilité (nouvellement créé par l’utilisateur) | Galerie | Désactivé avec « Oui » comme valeur par défaut |
| Groupe à haute disponibilité (nouvellement créé par l’utilisateur) | Stockage d'objets blob | Activé avec « Non » comme valeur par défaut |
