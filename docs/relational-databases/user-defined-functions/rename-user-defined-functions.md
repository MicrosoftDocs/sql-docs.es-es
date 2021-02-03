---
description: Cambiar el nombre de las funciones definidas por el usuario
title: Cambio de nombre de las funciones definidas por el usuario | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: c2695a5c-9cc5-4b18-8771-53027ca9a9af
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 835ba74b8bcd50692b7d74cbdf3e94fc501c163d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184250"
---
# <a name="rename-user-defined-functions"></a>Cambiar el nombre de las funciones definidas por el usuario
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Puede cambiar el nombre de las funciones definidas por el usuario en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para cambiar el nombre de las funciones definidas por el usuario, con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Los nombres de las funciones deben ajustarse a las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md).  
  
-   Al cambiar el nombre de una función definida por el usuario no se cambiará el nombre del objeto correspondiente en la columna de definición de la vista de catálogo **sys.sql_modules** . Por tanto, se recomienda no cambiar este tipo de objeto. En su lugar, quite el procedimiento almacenado y vuelva a crearlo con su nuevo nombre.  
  
-   El hecho de cambiar el nombre o la definición de una función definida por el usuario puede provocar errores en los objetos dependientes si no se actualizan con arreglo a los cambios realizados en la función.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Para quitar la función, se requiere el permiso ALTER en el esquema al que pertenece la función o un permiso CONTROL en la función. Para volver a crear la función, se requiere el permiso CREATE FUNCTION en la base de datos y el permiso ALTER en el esquema en el que se va a crear la función.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-rename-user-defined-functions"></a>Para cambiar el nombre de las funciones definidas por el usuario  
  
1.  En el **Explorador de objetos**, haga clic en el signo más situado junto a la base de datos que contiene la función a la que desea cambiar el nombre y  
  
2.  Haga clic en el signo más junto a la carpeta **Programación** .  
  
3.  Haga clic en el signo más junto a la carpeta que contenga la función cuyo nombre desea cambiar:  
  
    -   Table-valued Function  
  
    -   Función con valor escalar  
  
    -   Función de agregado  
  
4.  Haga clic con el botón derecho en la función cuyo nombre quiere cambiar y seleccione **Cambiar nombre**.  
  
5.  Escriba el nuevo nombre de la función.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 **Para cambiar el nombre de las funciones definidas por el usuario**  
  
 Esta tarea no se puede realizar mediante instrucciones Transact-SQL. Para cambiar una función definida por el usuario mediante Transact-SQL, debe eliminar la función existente y volver a crearla con el nuevo nombre. Asegúrese de que todo el código y las aplicaciones que usaban el nombre antiguo de la función usan el nuevo.  
  
 Para obtener más información, vea [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md) y [DROP FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-function-transact-sql.md).  
  
## <a name="see-also"></a>Consulte también  
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)   
 [Ver funciones definidas por el usuario](../../relational-databases/user-defined-functions/view-user-defined-functions.md)  
  
  
