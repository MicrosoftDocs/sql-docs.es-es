---
description: Requerir valores de atributo (Master Data Services)
title: Requerir valores de atributo
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], requiring attribute values
- attributes [Master Data Services], requiring values
ms.assetid: a360ef13-0c34-43b8-a87e-2f5d8732d30e
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 8e863605632b82b18f0dbe824fd45adbeaf22274
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353774"
---
# <a name="require-attribute-values-master-data-services"></a>Requerir valores de atributo (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], requiera valores de atributo cuando desee asegurarse de que los datos maestros están completos.  
  
> [!NOTE]  
>  Los miembros que sean valores de atributos basados en dominio y que falten no se muestran en las jerarquías derivadas que se basen en esas relaciones.  
  
## <a name="prerequisites"></a>Prerrequisitos  
 Para realizar este procedimiento:  
  
-   Debe disponer de permiso para tener acceso al área funcional de **Administración del sistema** .  
  
-   Debe ser administrador de modelo. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-require-attribute-values"></a>Requerir los valores de atributo  
  
1.  En [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], haga clic en **Administración del sistema**.  
  
2.  En la barra de menús, seleccione **Administrar** y haga clic en **Reglas de negocios**.  
  
3.  En la página **reglas de negocios** , en la lista desplegable **modelo** , seleccione un modelo.  
  
4.  En la lista desplegable **Entidad** , seleccione una entidad.  
  
5.  En la lista desplegable **Member Types** (Tipos de miembros), seleccione un tipo de miembro al que aplicar la regla de negocio.  
  
6.  Haga clic en **Agregar**.  
  
7.  En el cuadro **Nombre** , escriba un nombre para la regla de negocio.  
  
8.  Opcionalmente, en el campo **Descripción** , escriba la descripción de la regla de negocio.  
  
9. En el bloque **Entonces** , haga clic en **Agregar**. Se mostrará un panel.  
  
10. En la lista desplegable **Operador** , seleccione una **acción necesaria**.  
  
11. En la lista desplegable **Atributo** , seleccione un atributo.  
  
12. Haga clic en **Save**(Guardar). Se agregará una fila nueva a la cuadrícula **Entonces** .  
  
13. Haga clic en **Save**(Guardar).  
  
14. Haga clic en **Publish All**(Publicar todo).  
  
15. En el cuadro de diálogo de confirmación, haga clic en **Aceptar**. El valor de la columna **Business Rule State** (Estado de la regla de negocio) es **Activo**.  
  
## <a name="next-steps"></a>Pasos siguientes  
  
-   Aplique las reglas de negocios a los datos siguiendo uno de estos procedimientos:  
  
    -   [Validar miembros específicos con las reglas de negocios &#40;Master Data Services&#41;](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)  
  
    -   [Validar una versión con las reglas de negocios &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)  
  
## <a name="see-also"></a>Consulte también  
 [Reglas de negocios &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)   
 [Jerarquías derivadas &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md)  
  
  
