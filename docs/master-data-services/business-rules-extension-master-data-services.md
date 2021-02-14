---
title: Business Rules Extension
description: Puede aplicar scripts SQL definidos por el usuario como una extensión de condiciones y acciones de reglas de negocios predefinidas en Master Data Services.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 4c18be5f-a3fa-45a8-9be6-0f45f58bbc9e
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 0f99b4496f8abaae384c67d2c7849687f7045744
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341498"
---
# <a name="business-rules-extension-master-data-services"></a>Extensión de reglas de negocios (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], puede aplicar scripts SQL definidos por el usuario como una extensión de las acciones y las condiciones predefinidas.  
  
> [!NOTE]  
>  Todos los scripts deben definirse en el esquema [usr].  
  
 Las funciones de SQL que cumplen los criterios siguientes pueden utilizarse como una condición de regla de negocio.  
  
-   El tipo de valor devuelto debe ser de BIT.  
  
-   Solo se admiten los tipos siguientes para los tipos de parámetro.  
  
    -   NVARCHAR  
  
    -   DATETIME2  
  
    -   DECIMAL (precisión, escala)  
  
         la precisión debe ser 38  
  
         la escala debe ser un valor comprendido entre 0 y 7  
  
 Los procedimientos almacenados de SQL que utilizan la sintaxis siguiente pueden utilizarse como una acción de regla de negocio.  
  
```  
CREATE PROCEDURE [usr].[YourAction]  
       (         
            @MemberIdList mdm.[MemberId] READONLY,  
            @ModelName NVARCHAR(MAX),  
            @VersionName NVARCHAR(MAX),  
            @EntityName NVARCHAR(MAX),  
            @BusinessRuleName NVARCHAR(MAX)  
        )    
      AS BEGIN    
       ...     
       END  
  
```  
  
 Los scripts definidos por el usuario no se agregarán a los paquetes de implementación. Asegúrese de que la base de datos de Master Data Services de destino contiene todos los scripts que se utilizan en las reglas de negocios antes de implementar un paquete.  
  
 Las acciones de script se ejecutarán como mds_br_user con los permisos siguientes.  
  
|Schema|Permisos|  
|-|-|  
|mdm|SELECT|  
|stg|SELECT, UPDATE, DELETE, EXECUTE, INSERT|  
|usr|FULL|  
  
## <a name="prerequisites"></a>Requisitos previos  
 Para realizar este procedimiento:  
  
-   Debe disponer de permiso para tener acceso al área funcional de Administración del sistema.  
  
-   Debe ser administrador de modelo. Para obtener más información, consulte [Administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)  
  
-   Los scripts definidos por el usuario se agregan a la base de datos [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
## <a name="create-a-business-rule-to-take-a-user-defined-script-as-a-condition-or-as-an-action"></a>Creación de una regla de negocio para establecer un script definido por el usuario como una condición o una acción  
  
1.  En Master Data Manager, haga clic en **Administración del sistema**.  
  
2.  En la barra de menús, seleccione **Administrar** y haga clic en **Reglas de negocios**.  
  
3.  En la página **Reglas de negocios** , seleccione un modelo de la lista desplegable **Modelo** .  
  
4.  En la lista desplegable **Entidad** , seleccione una entidad.  
  
5.  En la lista desplegable **Member Types** (Tipos de miembros), seleccione un tipo de miembro al que aplicar la regla de negocio.  
  
6.  Haga clic en **Agregar**.  
  
7.  Siga este procedimiento para crear un script definido por el usuario como una condición.  
  
    1.  En el bloque **Sí** , haga clic en el botón **Agregar** . Se mostrará un panel.  
  
    2.  En la lista desplegable **Operador** , seleccione la función definida por el usuario en **Script definido por el usuario** .  
  
    3.  Se muestran todos los parámetros de la función definida por el usuario.  
  
    4.  Asigne un valor a cada parámetro.  
  
    5.  Haga clic en **Save**(Guardar).  
  
8.  Haga lo siguiente para establecer un script definido por el usuario como una acción  
  
    1.  En el bloque **Entonces** , haga clic en el botón **Agregar** . Se mostrará un panel.  
  
    2.  En la lista desplegable **Operador** , seleccione la función definida por el usuario en **Script definido por el usuario** .  
  
    3.  Haga clic en **Save**(Guardar).  
  
## <a name="see-also"></a>Consulte también  
 [Reglas de negocios &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)   
 [Condiciones de reglas de negocios &#40;Master Data Services&#41;](../master-data-services/business-rule-conditions-master-data-services.md)   
 [Acciones de reglas de negocios &#40;Master Data Services&#41;](../master-data-services/business-rule-actions-master-data-services.md)  
  
  
