---
description: Aplicar reglas de negocios (complemento MDS para Excel)
title: Aplicar reglas de negocios
ms.custom: microsoft-excel-add-in
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: cd106345-f561-4966-88d3-a69139b2bd78
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 6ac9299f7d1b8b436a1ccbbe06eef16df9ea3e7c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341439"
---
# <a name="apply-business-rules-mds-add-in-for-excel"></a>Aplicar reglas de negocios (complemento MDS para Excel)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En el [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)] , puede aplicar reglas de negocios cuando desee validar datos y confirmar que son válidos. Puede corregir validaciones y volver a publicar los datos.  
  
> [!NOTE]  
>  La validación de datos aparece automáticamente al publicar datos. Para obtener más información, consulte [Validación &#40;Master Data Services&#41;](../../master-data-services/validation-master-data-services.md).  
  
## <a name="prerequisites"></a>Prerrequisitos  
 Para realizar este procedimiento:  
  
-   Debe tener acceso al área funcional **Explorador** .  
  
-   Debe tener una hoja de cálculo activa que contenga datos administrados por MDS.  
  
### <a name="to-apply-business-rules"></a>Para aplicar reglas de negocios  
  
1.  En el grupo **Publicar y validar** , haga clic **Aplicar las reglas**.  
  
    > [!NOTE]  
    >  El número de miembros (filas) que se validan al mismo tiempo depende de un valor de [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)]. Para más información, consulte [Business Rule Settings](../../master-data-services/system-settings-master-data-services.md#BusinessRules).  
  
2.  Los datos se validan comparándolos con las reglas de negocios y se muestran dos columnas de estado. Si estas columnas no se muestran automáticamente, en el grupo **Publicar y validar** , haga clic en **Mostrar estado** para verlas.  
  
## <a name="see-also"></a>Consulte también  
 [Información general: Importación de datos desde Excel &#40;complemento MDS para Excel&#41;](../../master-data-services/microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md)  
  
  
