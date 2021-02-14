---
description: Importación de datos de Excel en Master Data Services (complemento MDS para Excel)
title: Importar datos de Excel
ms.custom: microsoft-excel-add-in, seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 89fce454-a816-4b33-a26a-d1b9741d269b
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 22d69fafa852007c9642fe641205185115b42175
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352757"
---
# <a name="import-data-from-excel-to-master-data-services-mds-add-in-for-excel"></a>Importación de datos de Excel en Master Data Services (complemento MDS para Excel)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)], puede publicar los datos en el repositorio MDS cuando termine de trabajar en Excel y desee guardar los cambios para que otros usuarios tengan acceso a ellos.  
  
> [!NOTE]
>  -   Al publicar los cambios, se eliminan los comentarios de las celdas administradas por MDS.  
> -   Una fórmula no se admite en una celda administrada por MDS. Una fórmula en una celda administrada por MDS se trata como un valor de texto.  
  
## <a name="prerequisites"></a>Requisitos previos  
 Para realizar este procedimiento:  
  
-   Debe disponer de permiso de acceso al área funcional **Explorador** .  
  
-   La hoja de cálculo activa debe contener datos administrados por MDS y debe haber realizado cambios o adiciones a los datos administrados por MDS.  
  
-   Si agrega miembros, no tiene que especificar un valor de **Código** si los códigos para la entidad se generan automáticamente. Para obtener más información, consulte [creación automática de código &#40;Master Data Services&#41;](../../master-data-services/automatic-code-creation-master-data-services.md).  
  
### <a name="to-publish-data-to-the-mds-repository"></a>Publicar datos en el repositorio MDS  
  
1.  En el grupo **Publicar y validar** , haga clic en **Publicar**.  
  
2.  Opcional. Si aparece el cuadro de diálogo **Publicar y anotar** , elija compartir la misma anotación (comentario) para todas las actualizaciones o anotar cada cambio de forma individual.  
  
3.  Opcional. Active la casilla **No volver a mostrar este cuadro de diálogo** . Puede mostrar siempre el cuadro de diálogo en el futuro si elije **Configuración** y activa la casilla **Mostrar el cuadro de diálogo Publicar y anotar al publicar** .  
  
4.  Haga clic en **Publicar**.  
  
> [!NOTE]  
>  Si agrega nuevos miembros (filas) en la hoja de cálculo y no puede publicarlos correctamente en el repositorio MDS, es posible que no tenga el permiso **Actualizar** para todos los atributos de la hoja de cálculo. En la pestaña **Revisar** , en el grupo **Cambios** , haga clic en **Desproteger hoja** e intente publicarla de nuevo.  
  
## <a name="next-steps"></a>Pasos siguientes  
 [Aplicar reglas de negocios &#40;complemento MDS para Excel&#41;](../../master-data-services/microsoft-excel-add-in/apply-business-rules-mds-add-in-for-excel.md)  
  
## <a name="see-also"></a>Consulte también  
 [Información general: importar datos desde Excel &#40;Complemento MDS para Excel&#41;](../../master-data-services/microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md)   
 [Validar datos &#40;complemento MDS para Excel&#41;](../../master-data-services/microsoft-excel-add-in/validating-data-mds-add-in-for-excel.md)  
  
  
