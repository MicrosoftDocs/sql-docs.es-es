---
description: Generar automáticamente valores de atributo que no sean Code (Master Data Services)
title: Generar automáticamente valores de atributo
titleSuffix: Master Data Services
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: b82f6f81-6e9c-4918-9ea9-4ab5f5d11b15
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 90302e701b8975d300e77cc7ca028c76825a79f6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339551"
---
# <a name="automatically-generate-attribute-values-other-than-code-master-data-services"></a>Generar automáticamente valores de atributo que no sean Code (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], genere de forma automática los valores de atributo de una entidad si quiere que se asigne automáticamente un entero como valor cada vez que se apliquen reglas de negocio.  
  
## <a name="prerequisites"></a>Requisitos previos  
 Para realizar este procedimiento:  
  
-   Debe disponer de permiso para tener acceso al área funcional de **Administración del sistema** .  
  
-   Debe ser administrador de modelo. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Debe existir un atributo numérico. Para obtener más información, consulte [Crear un atributo numérico &#40;Master Data Services&#41;](../master-data-services/create-a-numeric-attribute-master-data-services.md).  
  
### <a name="to-automatically-generate-an-attribute-value"></a>Para generar automáticamente un valor de atributo  
  
1.  En [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], haga clic en **Administración del sistema**.  
  
2.  En la barra de menús, seleccione **Administrar** y haga clic en **Reglas de negocios**.  
  
3.  En la página **Mantenimiento de reglas de negocios** , en la lista **Modelo** , seleccione un modelo.  
  
4.  En la lista **Entidad** , seleccione una entidad.  
  
5.  En la lista **Tipo de miembro** , seleccione un tipo de miembro al que aplicar la regla de negocios.  
  
6.  En la lista **Atributo** , deje el valor predeterminado **Todos**.  
  
7.  Haga clic en **Agregar regla de negocios**.  
  
8.  Haga clic en **Editar regla de negocios seleccionada**.  
  
9. En el panel **Componentes** , expanda el nodo **Acciones** .  
  
10. En el nodo de valor predeterminado, haga clic en **tiene como valor predeterminado un valor generado** y arrástrelo hasta la etiqueta de **Acciones** del panel **THEN**.  
  
11. En el panel **Atributos** , haga clic en un atributo que desea generar y arrástrelo hasta la etiqueta **Seleccionar atributo** del panel **Editar acción** .  
  
12. Escriba un valor en los cuadros **Empezar con** e **Incrementar en** . Si ya existen miembros, el valor se establece según el mayor valor existente. Por ejemplo, si el mayor valor existente es 299 e **Incrementar en** se ha establecido en **1**, el valor del miembro siguiente se establecerá en 300.  
  
13. En el panel **Editar acción** , haga clic en **Guardar elemento**.  
  
14. Haga clic en **Atrás**.  
  
15. Opcionalmente, en la página **Business Rules Maintenance** (Mantenimiento de las reglas de negocios), en la fila que contiene la regla de negocio, haga doble clic en una celda de las columnas **Nombre**, **Descripción** o **Notificación** para actualizar el valor.  
  
16. Haga clic en **Publicar reglas de negocios**.  
  
17. En el cuadro de diálogo de confirmación, haga clic en **Aceptar**. El estado de la regla cambia a **Activo**.  
  
## <a name="next-steps"></a>Pasos siguientes  
  
-   [Validar miembros específicos con las reglas de negocios &#40;Master Data Services&#41;](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)  
  
-   [Validar una versión con las reglas de negocios &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)  
  
## <a name="see-also"></a>Consulte también  
 [Creación automática de código &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md)   
 [Reglas de negocios &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)   
 [Validación &#40;Master Data Services&#41;](../master-data-services/validation-master-data-services.md)  
  
  
