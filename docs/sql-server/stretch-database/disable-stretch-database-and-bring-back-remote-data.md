---
description: Deshabilitación de Stretch Database y devolución de datos remotos
title: Deshabilitación de Stretch Database y devolución de datos remotos
ms.date: 08/05/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, disabling
- disabling Stretch Database
ms.assetid: c1bbb24e-47e3-46aa-b786-fcadf9fb65ce
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 315cfed40b1ccdadf349d9b371d6baac5928d3af
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081078"
---
# <a name="disable-stretch-database-and-bring-back-remote-data"></a>Deshabilitación de Stretch Database y devolución de datos remotos
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Para deshabilitar Stretch Database para una tabla, seleccione **Stretch** para una tabla en SQL Server Management Studio. Seleccione una de las siguientes opciones.  
  
-   **Deshabilitar | Devolver datos de Azure**. Copie los datos remotos de la tabla de Azure en SQL Server y luego deshabilite Stretch Database para la tabla. Esta operación provoca costos de transferencia de datos y no se puede cancelar.  
  
-   **Deshabilitar | Dejar los datos en Azure**. Deshabilite Stretch Database para la tabla.  Abandone los datos remotos para la tabla en Azure.  
  
 También puede usar Transact-SQL para deshabilitar Stretch Database para una tabla o una base de datos.  
  
 Después de deshabilitar Stretch Database para una tabla, se detiene la migración de datos y los resultados de la consulta dejan de incluir los resultados de la tabla remota.  
  
 Si solo quiere pausar la migración de datos, vea [Pause and resume data migration &#40;Stretch Database&#41; (Pausar y reanudar la migración de datos &#40;Stretch Database&#41;)](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md).  
  
> [!NOTE]
> Al deshabilitar la característica Stretch Database para una tabla o una base de datos, no se elimina el objeto remoto. Si quiere eliminar la tabla o la base de datos remotas, tiene que quitarlas mediante el Portal de administración de Azure. Los objetos remotos siguen acumulando costos de Azure hasta que se eliminan. Para obtener más información, consulte [Precios SQL Server Stretch Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
  
## <a name="disable-stretch-database-for-a-table"></a>Deshabilitar Stretch Database para una tabla  
  
### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-table"></a>Para deshabilitar Stretch Database para una tabla, use SQL Server Management Studio.  
  
1.  En SQL Server Management Studio, en el Explorador de objetos, seleccione la tabla para la que quiere deshabilitar Stretch Database.  
  
2.  Haga clic con el botón derecho y seleccione **Stretch** y, luego, seleccione una de las siguientes opciones.  
  
    -   **Deshabilitar | Devolver datos de Azure**. Copie los datos remotos de la tabla de Azure en SQL Server y luego deshabilite Stretch Database para la tabla. Este comando no se puede cancelar.  
  
        > [!NOTE]
        > La copia de los datos remotos para la tabla de Azure en SQL Server conlleva gastos de transferencia de datos. Para obtener más información, consulte [Detalles de precios de Transferencias de datos](https://azure.microsoft.com/pricing/details/data-transfers/).  
  
         Una vez que todos los datos remotos se han copiado desde Azure en SQL Server, se deshabilita Stretch para la tabla.  
  
    -   **Deshabilitar | Dejar los datos en Azure**. Deshabilite Stretch Database para la tabla.  Abandone los datos remotos para la tabla en Azure.  
  
    > [!NOTE]
    > Al deshabilitar Stretch Database para una tabla no se eliminan los datos remotos ni la tabla remota. Si quiere eliminar la tabla remota, tiene que quitarla mediante el Portal de administración de Azure. La tabla remota sigue acumulando costos de Azure hasta que se elimina. Para obtener más información, consulte [Precios SQL Server Stretch Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
  
### <a name="use-transact-sql-to-disable-stretch-database-for-a-table"></a>Usar Transact-SQL para deshabilitar Stretch Database para una tabla  
  
-   Para deshabilitar Stretch para una tabla y copiar los datos remotos de la tabla de Azure en SQL Server, ejecute el siguiente comando. Una vez que todos los datos remotos se han copiado desde Azure en SQL Server, se deshabilita Stretch para la tabla.

    Este comando no se puede cancelar.  
  
    ```sql  
    USE <Stretch-enabled database name>;
    GO
    ALTER TABLE <Stretch-enabled table name>  
       SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ; 
    GO 
    ```  
  
    > [!NOTE]
    > La copia de los datos remotos para la tabla de Azure en SQL Server conlleva gastos de transferencia de datos. Para obtener más información, consulte [Detalles de precios de Transferencias de datos](https://azure.microsoft.com/pricing/details/data-transfers/).    
  
-   Para deshabilitar Stretch para una tabla y abandonar los datos remotos, ejecute el siguiente comando.  
  
    ```sql  
    USE <Stretch-enabled database name>;
    GO
    ALTER TABLE <Stretch-enabled table name>  
       SET ( REMOTE_DATA_ARCHIVE = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED ) ) ; 
    GO
    ```  
  
> [!NOTE]
> Al deshabilitar Stretch Database para una tabla no se eliminan los datos remotos ni la tabla remota. Si quiere eliminar la tabla remota, tiene que quitarla mediante el Portal de administración de Azure. La tabla remota sigue acumulando costos de Azure hasta que se elimina. Para obtener más información, consulte [Precios SQL Server Stretch Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
  
## <a name="disable-stretch-database-for-a-database"></a>Deshabilitación de Stretch Database para una base de datos  
 Para poder deshabilitar Stretch Database para una base de datos, tiene que deshabilitar Stretch Database en las tablas individuales habilitadas para Stretch de la base de datos.  
  
### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-database"></a>Uso de SQL Server Management Studio para deshabilitar Stretch Database para una base de datos  
  
1.  En SQL Server Management Studio, en el Explorador de objetos, seleccione la base de datos para la que quiere deshabilitar Stretch Database.  
  
2.  Haga clic con el botón derecho y seleccione **Tareas**, luego **Stretch** y, por último, **Deshabilitar**.  
  
> [!NOTE]
> Al deshabilitar Stretch Database para una base de datos no se elimina la base de datos remota. Si quiere eliminar la base de datos remota, debe hacerlo mediante el Portal de administración de Azure. La base de datos remota sigue acumulando costos de Azure hasta que se elimina. Para obtener más información, consulte [Precios SQL Server Stretch Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
  
### <a name="use-transact-sql-to-disable-stretch-database-for-a-database"></a>Usar Transact-SQL para deshabilitar Stretch Database para una base de datos  
 Ejecute el siguiente comando.  
  
```sql  
ALTER DATABASE <Stretch-enabled database name>  
    SET REMOTE_DATA_ARCHIVE = OFF ;  
GO 
```  
  
> [!NOTE]
> Al deshabilitar Stretch Database para una base de datos no se elimina la base de datos remota. Si quiere eliminar la base de datos remota, debe hacerlo mediante el Portal de administración de Azure. La base de datos remota sigue acumulando costos de Azure hasta que se elimina. Para obtener más información, consulte [Precios SQL Server Stretch Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
  
## <a name="see-also"></a>Consulte también  
 [Opciones de ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [Pause and resume data migration &#40;Stretch Database&#41; (Pausar y reanudar la migración de datos &#40;Stretch Database&#41;)](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md)  
  
  
