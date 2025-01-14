---
title: Gérer une demande de support Azure
description: Décrit les procédures d’affichage des demandes de support, d’envoi de messages, de modification du niveau de gravité de la demande, de partage des informations de diagnostic avec le support Azure, de réouverture d’une demande de support clôturée et de chargement de fichiers.
tags: billing
ms.assetid: 86697fdf-3499-4cab-ab3f-10d40d3c1f70
ms.topic: how-to
ms.date: 05/25/2021
ms.openlocfilehash: bc9edecd918668e76c36308a660c47d3a8fd9e8b
ms.sourcegitcommit: bb9a6c6e9e07e6011bb6c386003573db5c1a4810
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/26/2021
ms.locfileid: "110496512"
---
# <a name="manage-an-azure-support-request"></a>Gérer une demande de support Azure

Après avoir [créé une demande de support Azure](how-to-create-azure-support-request.md), vous pouvez la gérer dans le [portail Azure](https://portal.azure.com), comme décrit dans cet article. Vous pouvez également créer et gérer des demandes par programmation, en utilisant l’[API REST de ticket de support Azure](/rest/api/support) ou [Azure CLI](/cli/azure/azure-cli-support-request).

## <a name="view-support-requests"></a>Afficher les demandes de support

Pour afficher les détails et l’état des demandes de support, accédez à **Aide + support** >  **Toutes les demandes de support**.

:::image type="content" source="media/how-to-manage-azure-support-request/all-requests-lower.png" alt-text="Toutes les demandes de support":::

Sur cette page, vous pouvez rechercher, filtrer et trier des demandes de support. Sélectionnez une demande de support pour afficher les détails, notamment la sévérité et les messages associés à la demande.

## <a name="send-a-message"></a>Envoyer un message

1. Sur la page **Toutes les demandes de support**, sélectionnez la demande de support.

1. Sur la page **Demande de support**, sélectionnez **Nouveau message**.

1. Entrez votre message et sélectionnez **Envoyer**.

## <a name="change-the-severity-level"></a>Modifier le niveau de gravité

> [!NOTE]
> Le niveau de gravité maximale dépend de votre [plan de support](https://azure.microsoft.com/support/plans).
>

1. Sur la page **Toutes les demandes de support**, sélectionnez la demande de support.

1. Sur la page **Demande de support**, sélectionnez **Modifier**.

    :::image type="content" source="media/how-to-manage-azure-support-request/change-severity.png" alt-text="Modifier la gravité de la demande de support":::

1. Le portail Azure affiche l’un ou l’autre écran, selon que votre demande est déjà attribuée à un ingénieur du support technique ou non :

    - Si votre demande n’a pas été attribuée, un écran similaire à celui ci-dessous s’affiche. Sélectionnez un nouveau niveau de gravité, puis sélectionnez **Modifier**.

        :::image type="content" source="media/how-to-manage-azure-support-request/unassigned-can-change-severity.png" alt-text="Sélectionner un nouveau niveau de gravité":::

    - Si votre demande a été attribuée, un écran similaire à celui ci-dessous s’affiche. Sélectionnez **OK**, puis créez un [message](#send-a-message) pour demander une modification du niveau de gravité.

        :::image type="content" source="media/how-to-manage-azure-support-request/assigned-cant-change-severity.png" alt-text="Impossible de sélectionner un nouveau niveau de gravité":::

## <a name="share-diagnostic-information-with-azure-support"></a>Partager des informations de diagnostic avec le support Azure

Lors de la création d’une requête de support, l’option **Partager les informations de diagnostic** est sélectionnée par défaut. Cette option permet au support Azure de collecter des [informations de diagnostic](https://azure.microsoft.com/support/legal/support-diagnostic-information-collection/) à partir de vos ressources Azure qui peuvent éventuellement vous aider à résoudre votre problème.

Pour modifier la sélection des **informations de diagnostic de partage** après la création de la requête :

1. Sur la page **Toutes les demandes de support**, sélectionnez la demande de support.
    
1. Sur la page **Demande de support**, recherchez **Informations de diagnostic de partage**, puis sélectionnez **Modifier**.
    
1.  Sélectionnez **Oui** ou **Non**, puis cliquez sur **OK** pour confirmer.
    
    :::image type="content" source="media/how-to-manage-azure-support-request/grant-permission-manage.png" alt-text="Accorder des autorisations pour les informations de diagnostic":::

## <a name="upload-files"></a>Charger des fichiers

Vous pouvez utiliser l’option de chargement de fichiers de diagnostic ou tout autre fichier qui vous semble s’appliquer à la demande de support.

1. Sur la page **Toutes les demandes de support**, sélectionnez la demande de support.

1. Sur la page **Demande de support**, recherchez votre fichier, puis sélectionnez **Charger**. Répétez la procédure si vous avez plusieurs fichiers.

    :::image type="content" source="media/how-to-manage-azure-support-request/file-upload.png" alt-text="Charger un fichier":::

### <a name="file-upload-guidelines"></a>Instructions relatives au chargement de fichiers

Suivez ces instructions lorsque vous utilisez l’option de chargement de fichiers :

* Pour protéger votre confidentialité, évitez d’inclure des informations personnelles dans votre téléchargement.
* Le nom de fichier doit être inférieur à 110 caractères.
* Vous ne pouvez pas charger plusieurs fichiers.
* Les fichiers ne peuvent pas dépasser 4 Mo.
* Tous les fichiers doivent avoir une extension de nom de fichier, telle que  *.docx* ou  *.xlsx*. Le tableau suivant indique les extensions de nom de fichier dont le chargement est autorisé.

| 0-9, A-C    | D-G   | H-N         | O-Q   | R-T      | U-W        | X-Z     |
|-------------|-------|-------------|-------|----------|------------|---------|
| .7Z         | .dat  | .har        | .odx  | .rar     | .uccapilog | .xlam   |
| .a          | .db   | .hwl        | .oft  | .rdl     | .uccplog   | .xlr    |
| .abc        | .DMP  | .ics        | .old  | .rdlc    | .udcx      | .xls    |
| .adm        | .do_  | .ini        | .one  | .re_     | .vb_       | .xlsb   |
| .aspx       | .doc  | .java       | .osd  | .remove  | .vbs_      | .xlsm   |
| .ATF        | .docm | .jpg        | .OUT  | .ren     | .vcf       | .xlsx   |
| .b          | .docx | .LDF        | .p1   | .rename  | .vsd       | .xlt    |
| .ba_        | .dotm | .letterhead | .pcap | .rft     | .wdb       | .xltx   |
| .bak        | .dotx | .lo_        | .pdb  | .rpt     | .wks       | .xml    |
| .blg        | .dtsx | .log        | .pdf  | .rte     | .wma       | .xmla   |
| .CA_        | .eds  | .lpk        | .piz  | .rtf     | .wmv       | .xps    |
| .CAB        | .emf  | .manifest   | .pmls | .run     | .wmz       | .xsd    |
| .cap        | .eml  | .master     | .png  | .saz     | .wps       | .xsn    |
| .catx       | .emz  | .mdmp       | .potx | .sql     | .wpt       | .xxx    |
| .CFG        | .err  | .mof        | .ppt  | .sqlplan | .wsdl      | .z_     |
| .compressed | .etl  | .mp3        | .pptm | .stp     | .wsp       | .z01    |
| .Config     | .evt  | .mpg        | .pptx | .svclog  | .wtl       | .z02    |
| .cpk        | .evtx | .ms_        | .prn  | .tdb     | -          | .zi     |
| .cpp        | .EX   | .msg        | .psf  | .tdf     | -          | .zi_    |
| .cs         | .ex_  | .mso        | .pst  | .text    | -          | .zip    |
| .CSV        | .ex0  | .msu        | .pub  | .thmx    | -          | .zip_   |
| .cvr        | .FRD  | .nfo        | -     | .tif     | -          | .zipp   |
| -           | .gif  | -           | -     | .trc     | -          | .zipped |
| -           | .guid | -           | -     | .TTD     | -          | .zippy  |
| -           | .gz   | -           | -     | .tx_     | -          | .zipx   |
| -           | -     | -           | -     | .txt     | -          | .zit    |
| -           | -     | -           | -     | -        | -          | .zix    |
| -           | -     | -           | -     | -        | -          | .zzz    |

## <a name="close-a-support-request"></a>Fermeture d’une demande de support

Si vous avez besoin de fermer une demande de support, [envoyez un message](#send-a-message) demandant de fermer la demande.

## <a name="reopen-a-closed-request"></a>Rouvrir une demande clôturée

Si vous devez rouvrir une demande de support clôturée, créez un [message](#send-a-message), qui rouvre automatiquement la demande.

## <a name="cancel-a-support-plan"></a>Annulation d’un plan de support

Si vous devez annuler un plan de support, consultez [Annulation d’un plan de support](../../cost-management-billing/manage/cancel-azure-subscription.md#cancel-a-support-plan).

## <a name="next-steps"></a>Étapes suivantes

[Création d’une demande de support Azure](how-to-create-azure-support-request.md)

[API REST de ticket de support Azure](/rest/api/support)
