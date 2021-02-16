---
title: Eliminación del testigo de creación de reflejo de la base de datos
description: Describe cómo quitar un testigo de una sesión de creación de reflejo de la base de datos con SQL Server Management Studio (SSMS) o Transact-SQL (T-SQL).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- witness [SQL Server], turning off
- witness [SQL Server], removing
- database mirroring [SQL Server], witness
ms.assetid: f3ce7afc-8936-4d35-80ce-d0f8fbc318d3
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 0f74b78045295dc045ea3bbf761adc7955bcdf76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337850"
---
# <a name="remove-the-witness-from-a-database-mirroring-session-sql-server"></a>Quitar el testigo de una sesión de creación de reflejo de la base de datos (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo quitar un testigo de una sesión de creación de reflejo de una base de datos en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. En cualquier momento durante una sesión de creación de reflejo de la base de datos, el propietario de la base de datos puede desactivar el testigo.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Seguridad](#Security)  
  
-   **Para quitar el testigo, mediante:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Seguimiento:**  [Después de quitar el testigo](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere el permiso ALTER en la base de datos.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-remove-the-witness"></a>Para quitar el testigo  
  
1.  Conéctese a la instancia de servidor principal y, en el panel **Explorador de objetos** , haga clic en el nombre del servidor para expandir el árbol de servidores.  
  
2.  Expanda **Bases de datos** y seleccione la base de datos cuyo testigo desee quitar.  
  
3.  Haga clic con el botón derecho en la base de datos, seleccione **Tareas** y, luego, haga clic en **Reflejado**. Así se abre la página **Creación de reflejo** del cuadro de diálogo **Propiedades de la base de datos** .  
  
4.  Para quitar el testigo, elimine su dirección de red del servidor del campo **Testigo** .  
  
    > [!NOTE]  
    >  Si pasa del modo de alta seguridad con conmutación automática por error al modo de alto rendimiento, el campo **Testigo** se vacía de forma automática.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-remove-the-witness"></a>Para quitar el testigo  
  
1.  Conéctese al [!INCLUDE[ssDE](../../includes/ssde-md.md)] en la instancia de servidor asociada.  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Emita la instrucción siguiente:  
  
     [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md) *nombre_base_de_datos* SET WITNESS OFF  
  
     Donde *nombre_base_de_datos* es el nombre de la base de datos reflejada.  
  
     En el ejemplo siguiente se quita el testigo de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET WITNESS OFF ;  
    ```  
  
##  <a name="follow-up-after-removing-the-witness"></a><a name="FollowUp"></a> Seguimiento: Después de quitar el testigo  
 Al desactivar el testigo cambia el [modo de funcionamiento](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)de acuerdo con la configuración de la seguridad de las transacciones:  
  
-   Si la seguridad de las transacciones está configurada en FULL (el valor predeterminado), la sesión utiliza el modo sincrónico de alta seguridad sin conmutación automática por error.  
  
-   Si la seguridad de las transacciones está configurada en OFF, el funcionamiento de la sesión es asincrónico (en modo de alto rendimiento) sin requerir quórum. Siempre que la seguridad de las transacciones se desactive, recomendamos desactivar también el testigo.  
  
> [!TIP]  
>  La configuración de seguridad de las transacciones de la base de datos está registrada en cada asociado de la vista de catálogo [sys.database_mirroring](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md) en las columnas **mirroring_safety_level** y **mirroring_safety_level_desc**.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Agregar un testigo de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)  
  
-   [Agregar o reemplazar un testigo de creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/add-or-replace-a-database-mirroring-witness-sql-server-management-studio.md)  
  
## <a name="see-also"></a>Consulte también  
 [Reflejo de la base de datos ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)   
 [Cambiar la seguridad de las transacciones en una sesión de creación de reflejo de la base de datos &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/change-transaction-safety-in-a-database-mirroring-session-transact-sql.md)   
 [Agregar un testigo de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)   
 [Testigo de creación de reflejo de la base de datos](../../database-engine/database-mirroring/database-mirroring-witness.md)  
  
  
