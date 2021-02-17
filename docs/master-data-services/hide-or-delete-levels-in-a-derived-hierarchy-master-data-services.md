---
description: Ocultar o eliminar niveles en una jerarquía derivada (Master Data Services)
title: Ocultar o eliminar niveles en una jerarquía derivada
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- derived hierarchies, hiding levels
- derived hierarchies, deleting levels
ms.assetid: e00582b9-9415-4b66-b4a7-9f590d83875f
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e1c63de9accf8b3de74793753f40ee2e01d0546b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344768"
---
# <a name="hide-or-delete-levels-in-a-derived-hierarchy-master-data-services"></a>Ocultar o eliminar niveles en una jerarquía derivada (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], oculte un nivel en una jerarquía derivada cuando requiera el nivel para agrupar, pero no necesite mostrarlo. Elimine un nivel cuando no desee usarlo para la agrupación.  
  
## <a name="prerequisites"></a>Prerrequisitos  
 Para realizar este procedimiento:  
  
-   Debe disponer de permiso para tener acceso al área funcional de **Administración del sistema** .  
  
-   Debe ser administrador de modelo. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-hide-or-delete-levels-in-a-derived-hierarchy"></a>Para ocultar o eliminar niveles en una jerarquía derivada  
  
1.  En [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], haga clic en **Administración del sistema**.  
  
2.  En la barra de menús, seleccione **Administrar** y haga clic en **Jerarquías derivadas**.  
  
3.  En la página **Mantenimiento de jerarquías derivadas** , en la lista **Modelo** , seleccione un modelo.  
  
4.  Seleccione la fila de la jerarquía derivada que desea modificar.  
  
5.  Haga clic en **Editar**.  
  
6.  En el panel **Niveles actuales** :  
  
    -   Para ocultar un nivel, haga clic en un nivel distinto de la parte superior o inferior. En la lista **Visible** , seleccione **No**. A continuación, haga clic en **Guardar elemento de jerarquía seleccionado**.  
  
    -   Para eliminar el nivel superior, haga clic en **Eliminar elemento de jerarquía seleccionado**. En el cuadro de diálogo de confirmación, haga clic en **Aceptar**. Solo puede eliminar el nivel superior.  
  
## <a name="see-also"></a>Consulte también  
    
 [Jerarquías derivadas &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md)  
  
  
