---
description: Enable Stretch Database for a table
title: Enable Stretch Database for a table
ms.date: 08/05/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, enabling table
- enabling table for Stretch Database
ms.assetid: de4ac0c5-46ef-4593-a11e-9dd9bcd3ccdc
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 29d4e1d6e2de7c575b9549f13dc325add4f5ca88
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081146"
---
# <a name="enable-stretch-database-for-a-table"></a>Enable Stretch Database for a table
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Si quiere configurar una tabla para Stretch Database, seleccione **Stretch | Habilitar** para una tabla de SQL Server Management Studio para el asistente **Habilitar la tabla para Stretch** . También puede utilizar Transact-SQL para habilitar Stretch Database en una tabla existente o crear una tabla nueva con Stretch Database habilitado.  
  
-   Si los datos inactivos están almacenados en otra tabla, puede migrarla entera.  
  
-   Si la tabla contiene datos activos e inactivos, puede especificar una función de filtro para seleccionar las filas que se migrarán.    
 
 **Requisitos previos**. Si selecciona **Stretch | Habilitar** para una tabla y aún no se ha habilitado Stretch Database para la base de datos, el asistente configura primero la base de datos para Stretch Database. Siga los pasos de [Introducción mediante la ejecución del Asistente para Habilitar base de datos para Stretch](../../sql-server/stretch-database/get-started-by-running-the-enable-database-for-stretch-wizard.md) en lugar de los pasos de este artículo.  
  
 **Permisos**. Para habilitar Stretch Database en una base de datos o tabla, se requieren permisos db_owner. La habilitación de Stretch Database en una tabla también requiere permisos ALTER en la tabla.  

 > [!NOTE]
 > Posteriormente, si deshabilita Stretch Database, recuerde que al deshabilitar Stretch Database para una tabla o una base de datos no se elimina el objeto remoto. Si quiere eliminar la tabla o la base de datos remotas, tiene que quitarlas mediante el Portal de administración de Azure. Los objetos remotos siguen acumulando gastos de Azure hasta que se eliminan manualmente.
 
##  <a name="use-the-wizard-to-enable-stretch-database-on-a-table"></a><a name="EnableWizardTable"></a> Uso del asistente para habilitar Stretch Database en una tabla  
 **Inicio del asistente**  
 1.  En SQL Server Management Studio, en el Explorador de objetos, seleccione la tabla en la que desea habilitar Stretch.  
  
2.  Haga clic con el botón derecho y seleccione **Stretch** y, después, seleccione **Habilitar** para iniciar el asistente.  
  
 **Introducción**  
 Consulte la finalidad del asistente y los requisitos previos.  
  
 **Selección de las tablas de base de datos**  
 Confirme que la tabla que desea habilitar se muestra y está seleccionada.  
  
 Puede migrar toda una tabla o especificar una función de filtro simple en el asistente. Si quiere usar un tipo de función de filtro diferente para seleccionar las filas que va a migrar, realice una de las siguientes acciones.  
  
-   Salga del asistente y ejecute la instrucción ALTER TABLE para habilitar Stretch para la tabla y especificar una función de filtro.  
  
-   Ejecute la instrucción ALTER TABLE para especificar una función de filtro después de salir del asistente. Para conocer los pasos necesarios, vea [Agregar una función de filtro después de ejecutar el asistente](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md#addafterwiz).  
  
 La sintaxis de ALTER TABLE se describe más adelante en este artículo.  
  
 **Resumen**  
 Revise los valores especificados y las opciones seleccionadas en el asistente. Después, seleccione **Finalizar** para habilitar Stretch.  
  
 **Resultados**  
 Revise los resultados.  
  
##  <a name="use-transact-sql-to-enable-stretch-database-on-a-table"></a><a name="EnableTSQLTable"></a> Uso de Transact-SQL para habilitar Stretch Database en una tabla  
 Puede utilizar Transact-SQL para habilitar Stretch Database en una tabla existente o crear una tabla nueva con Stretch Database habilitado.  
  
### <a name="options"></a>Opciones  
 Utilice las siguientes opciones al ejecutar CREATE TABLE o ALTER TABLE para habilitar Stretch Database en una tabla.  
  
-   Si la tabla contiene datos activos e inactivos, puede usar opcionalmente la cláusula `FILTER_PREDICATE = <function>` para especificar una función y seleccionar las filas que se migrarán. El predicado debe llamar a una función con valores de tabla insertada. Para obtener más información, vea [Select rows to migrate by using a filter function (Seleccionar las filas que se van a migrar mediante una función de filtro)](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md). Si no se especifica una función de filtro, se migrará toda la tabla.  
  
    > [!IMPORTANT]  
    > Si se indica una función de filtro que tiene un rendimiento bajo, la migración de datos también tendrá un rendimiento bajo. Stretch Database aplica la función de filtro a la tabla por medio del operador CROSS APPLY.  
  
-   Especifique `MIGRATION_STATE = OUTBOUND` para iniciar la migración de datos de inmediato o  `MIGRATION_STATE = PAUSED` si desea posponer el inicio de la migración.  
  
### <a name="enable-stretch-database-for-an-existing-table"></a>Habilitar Stretch Database para una tabla existente  
 Si desea configurar una tabla para Stretch Database, ejecute el comando ALTER TABLE.  
  
 En el ejemplo siguiente se migra toda la tabla y la migración de datos se inicia de inmediato.  
  
```sql  
USE <Stretch-enabled database name>;
GO
ALTER TABLE <table name>  
    SET ( REMOTE_DATA_ARCHIVE = ON ( MIGRATION_STATE = OUTBOUND ) ) ;  
GO
```  
  
 En el ejemplo siguiente solo se migran las filas identificadas con la función con valores de tabla insertada `dbo.fn_stretchpredicate` y se pospone la migración de datos. Para obtener más información sobre la función de filtro, vea [Select rows to migrate by using a filter function (Seleccionar las filas que se van a migrar mediante una función de filtro)](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).  
  
```sql  
USE <Stretch-enabled database name>;
GO
ALTER TABLE <table name>  
    SET ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(),  
        MIGRATION_STATE = PAUSED ) ) ;  
 GO
```  
  
 Para obtener más información, vea [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
### <a name="create-a-new-table-with-stretch-database-enabled"></a>Crear una nueva tabla con Stretch Database habilitado  
 Para crear una nueva tabla con Stretch Database habilitado, ejecute el comando CREATE TABLE.  
  
 En el ejemplo siguiente se migra toda la tabla y la migración de datos se inicia de inmediato.  
  
```sql  
USE <Stretch-enabled database name>;
GO
CREATE TABLE <table name>
    ( ... )  
    WITH ( REMOTE_DATA_ARCHIVE = ON ( MIGRATION_STATE = OUTBOUND ) ) ;  
GO
```  
  
 En el ejemplo siguiente solo se migran las filas identificadas con la función con valores de tabla insertada `dbo.fn_stretchpredicate` y se pospone la migración de datos. Para obtener más información sobre la función de filtro, vea [Select rows to migrate by using a filter function (Seleccionar las filas que se van a migrar mediante una función de filtro)](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).  
  
```sql  
USE <Stretch-enabled database name>;
GO
CREATE TABLE <table name> 
    ( ... )  
    WITH ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(),  
        MIGRATION_STATE = PAUSED ) ) ;  
GO  
```  
  
 Para obtener más información, vea [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
## <a name="see-also"></a>Consulte también  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
  
  
