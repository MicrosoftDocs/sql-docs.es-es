---
title: Movimiento de un grupo de cargas de trabajo | Microsoft Docs
description: Obtenga información sobre cómo mover un grupo de cargas de trabajo de Resource Governor a un grupo de recursos diferente mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.swb.rg.properties_moveworkloadgroup.f1
helpviewer_keywords:
- workload groups [SQL Server], move
- Resource Governor, workload group move
ms.assetid: f2068636-6e53-486a-a6fc-c12de2a38424
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 03ad7743638779aa1afc05359be45da377f997b7
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96506585"
---
# <a name="move-a-workload-group"></a>Mover un grupo de cargas de trabajo
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Puede mover un grupo de cargas de trabajo del regulador de recursos a un grupo de recursos de servidor diferente utilizando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o Transact-SQL.  
  
-   **Antes de empezar:**  [Limitaciones y restricciones](#LimitationsRestrictions), [Permisos](#Permissions)  
  
-   **Para mover un grupo de cargas de trabajo con:**  [SQL Server Management Studio](#MoveWGSSMS), [Transact-SQL](#MoveWGTSQL)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
 No puede mover un grupo de cargas de trabajo si hay una operación de configuración del regulador de recursos pendiente.  
  
###  <a name="limitations-and-restrictions"></a><a name="LimitationsRestrictions"></a> Limitaciones y restricciones  
 No puede mover un grupo de cargas de trabajo si hay una operación de configuración del regulador de recursos pendiente. Es posible determinar si existe una configuración pendiente consultando la vista de administración dinámica [sys.dm_resource_governor_configuration &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-configuration-transact-sql.md) para obtener el estado actual de is_configuration_pending.  
  
###  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Mover un grupo de cargas de trabajo requiere un permiso CONTROL SERVER.  
  
##  <a name="move-a-workload-group-using-sql-server-management-studio"></a><a name="MoveWGSSMS"></a> Mover un grupo de cargas de trabajo mediante SQL Server Management  
 **Para mover un grupo de cargas de trabajo utilizando [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]**  
  
1.  En el Explorador de objetos, expanda de forma recursiva el nodo **Administración** hasta el nodo **Regulador de recursos**.  
  
2.  Haga clic con el botón derecho en **Regulador de recursos** y, luego, haga clic en **Propiedades**. De este modo se abre la página **Propiedades del regulador de recursos** .  
  
3.  En la ventana **Grupos de recursos** , haga clic en el grupo de recursos de servidor que contiene el grupo de cargas de trabajo que se va a mover. La ventana **Grupos de cargas de trabajo** enumera ahora los grupos de cargas de trabajo que contiene ese grupo de recursos de servidor.  
  
4.  En la ventana **Grupos de cargas de trabajo** , haga clic con el botón derecho en la flecha derecha situada a la izquierda del grupo de cargas de trabajo que se va a mover y haga clic en **Mover a**. Esto muestra una ventana **Mover grupo de cargas de trabajo** .  
  
5.  Los grupos de recursos de servidor disponibles se muestran en la ventana. Haga clic en el nombre del grupo de recursos de servidor al que desea mover el grupo de cargas de trabajo y, a continuación, haga clic en **Aceptar** para llevar a cabo esta acción.  
  
6.  Esta acción no se completa hasta que se hace clic en **Aceptar**. Al hacer clic en **Aceptar**, se ejecutará la instrucción ALTER RESOURCE GOVERNOR RECONFIGURE.  
  
7.  Si la operación de creación o cambio de configuración sobre el grupo de recursos de servidor o el grupo de cargas de trabajo produce un error, aparecerá un informe de error debajo del título de la página de propiedades. Para ver un mensaje de error más detallado, haga clic en la flecha abajo que aparece en el mensaje de error.  
  
##  <a name="move-a-workload-group-using-transact-sql"></a><a name="MoveWGTSQL"></a> Mover un grupo de cargas de trabajo mediante Transact-SQL  
 **Para mover un grupo de cargas de trabajo utilizando Transact-SQL**  
  
1.  Ejecute la instrucción **ALTER WORKLOAD GROUP** especificando el nombre del grupo de cargas de trabajo que se va a mover y el grupo de recursos de servidor al que se debería mover.  
  
2.  Ejecute la instrucción **ALTER RESOURCE GOVERNOR RECONFIGURE** .  
  
### <a name="example-transact-sql"></a>Ejemplo (Transact-SQL)  
 El siguiente ejemplo mueve un grupo de cargas de trabajo denominado `groupAdhoc` al grupo de recursos de servidor predeterminado.  
  
```  
ALTER WORKLOAD GROUP groupAdhoc  
USING [default];  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Regulador de recursos](../../relational-databases/resource-governor/resource-governor.md)   
 [Habilitar el regulador de recursos](../../relational-databases/resource-governor/enable-resource-governor.md)   
 [Crear un grupo de recursos de servidor](../../relational-databases/resource-governor/create-a-resource-pool.md)   
 [Crear un grupo de cargas de trabajo](../../relational-databases/resource-governor/create-a-workload-group.md)   
 [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-workload-group-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  
