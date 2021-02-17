---
title: Bases de datos de ejemplo AdventureWorks
description: Siga estas instrucciones para descargar e instalar las bases de datos de ejemplo AdventureWorks para SQL Server mediante Transact-SQL (T-SQL), SQL Server Management Studio (SSMS) o Azure Data Studio.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.date: 06/16/2020
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: d706b9ef0ed5d95c411f00d426a8b9daf1415074
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340037"
---
# <a name="adventureworks-sample-databases"></a>Bases de datos de ejemplo AdventureWorks
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

En este artículo se proporcionan vínculos directos para descargar bases de datos de ejemplo de AdventureWorks, así como instrucciones para restaurarlas en SQL Server y Azure SQL Database. 

Para obtener más información sobre los ejemplos, vea el [repositorio de github de ejemplos](https://github.com/microsoft/sql-server-samples/tree/master/samples/databases). 

## <a name="prerequisites"></a>Prerrequisitos

- [SQL Server](https://www.microsoft.com/evalcenter/evaluate-sql-server-2019) o [Azure SQL Database](https://azure.microsoft.com/services/sql-database/)
- [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md) o [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)


## <a name="download-backup-files"></a>Descargar archivos de copia de seguridad 

Use estos vínculos para descargar la base de datos de ejemplo adecuada para su escenario. 

- Los datos **OLTP** son para las cargas de trabajo de procesamiento de transacciones en línea más habituales. 
- Los datos de **almacenamiento de datos (DW)** son para cargas de trabajo de almacenamiento de datos. 
- Los datos **ligeros (lt)** son una versión ligera y reducida de la muestra de **OLTP** . 

Si no está seguro de lo que necesita, comience con la versión de OLTP que coincida con la versión de SQL Server. 

|**OLTP** |**Almacenamiento de datos** |**Ligero**|
|---------|---------|---------|
|[AdventureWorks2019. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2019.bak)|[AdventureWorksDW2019. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2019.bak)|[AdventureWorksLT2019. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2019.bak)|
|[AdventureWorks2017. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2017.bak)|[AdventureWorksDW2017. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2017.bak)|[AdventureWorksLT2017. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2017.bak)|
|[AdventureWorks2016. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2016.bak)|[AdventureWorksDW2016. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2016.bak)|[AdventureWorksLT2016. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2016.bak)|
|[AdventureWorks2016_EXT. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2016_EXT.bak)|[AdventureWorksDW2016_EXT. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2016_EXT.bak)| N/D |
|[AdventureWorks2014. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2014.bak)|[AdventureWorksDW2014. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2014.bak)|[AdventureWorksLT2014. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2014.bak)|
|[AdventureWorks2012. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2012.bak)|[AdventureWorksDW2012. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2012.bak)|[AdventureWorksLT2012. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2012.bak)|
|[AdventureWorks2008R2. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks2008r2/adventure-works-2008r2-oltp.bak)| [AdventureWorksDW2008R2. bak](https://github.com/microsoft/sql-server-samples/releases/download/adventureworks2008r2/adventure-works-2008r2-dw.bak) | N/D |

Los archivos adicionales se pueden encontrar directamente en GitHub: 

- [SQL Server 2014-2019](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks)
- [SQL Server 2012](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks2012)
- [SQL Server 2008 y 2008R2](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks2008r2)


## <a name="restore-to-sql-server"></a>Restaurar en SQL Server 

Puede usar el `.bak` archivo para restaurar la base de datos de ejemplo en la instancia de SQL Server. Puede hacerlo mediante el comando [Restore (Transact-SQL)](../t-sql/statements/restore-statements-transact-sql.md) o mediante la interfaz gráfica (GUI) en [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md) o [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md).

# <a name="sql-server-management-studio-ssms"></a>[SQL Server Management Studio (SSMS)](#tab/ssms)

Si no está familiarizado con SQL Server Management Studio (SSMS), puede ver [connect & Query](../ssms/quickstarts/ssms-connect-query-sql-server.md) para comenzar. 

Para restaurar la base de datos en SQL Server Management Studio, siga estos pasos:

1. Descargue el `.bak` archivo adecuado de uno de los vínculos que se proporcionan en la sección [descargar archivos de copia de seguridad](#download-backup-files) .
2. Mueva el `.bak` archivo a la ubicación de copia de seguridad de SQL Server. Esto varía en función de la ubicación de instalación, el nombre de instancia y la versión de SQL Server. Por ejemplo, la ubicación predeterminada de una instancia predeterminada de SQL Server 2019 es:

   `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup`. 

3. Abra SQL Server Management Studio (SSMS) y conéctese a su SQL Server en. 
4. Haga clic con el botón secundario en **bases** de datos en **Explorador de objetos**  >  **restaurar base de datos...** para iniciar el Asistente para **restaurar bases de datos** . 

   :::image type="content" source="media/adventureworks-install-configure/restore-db-ssms.png" alt-text="Captura de pantalla que muestra cómo elegir cómo restaurar la base de datos haciendo clic con el botón secundario en bases de datos en Explorador de objetos y, a continuación, seleccionando restaurar base de datos.":::


1. Seleccione **dispositivo** y, a continuación, seleccione los puntos suspensivos **(...)** para elegir un dispositivo. 
1. Seleccione **Agregar** y, a continuación, elija el `.bak` archivo que acaba de migrar a esta ubicación. Si ha migrado el archivo a esta ubicación, pero no puede verlo en el asistente, esto suele indicar un problema de permisos-SQL Server o el usuario que inició sesión en SQL Server no tiene permiso para este archivo en esta carpeta. 
1. Seleccione **Aceptar** para confirmar la selección de la copia de seguridad de base de datos y cerrar la ventana **seleccionar dispositivos de copia de seguridad** . 
1. Compruebe la pestaña **archivos** para confirmar que la ubicación de la **restauración** y los nombres de archivo coinciden con los nombres de archivo y la ubicación que desea en el Asistente para **restaurar bases de datos** . 
1. Seleccione **Aceptar** para restaurar la base de datos. 

   :::image type="content" source="media/adventureworks-install-configure/restore-db-wizard-ssms.png" alt-text="Captura de pantalla que muestra la ventana restaurar base de datos con el conjunto de copia de seguridad para restaurar resaltado y la opción Aceptar.":::

Para obtener más información acerca de cómo restaurar una base de datos de SQL Server, consulte [restaurar una copia de seguridad de base de datos con SSMS](../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md).

# <a name="transact-sql-t-sql"></a>[Transact-SQL (T-SQL)](#tab/tsql)

Puede restaurar la base de datos de ejemplo mediante Transact-SQL (T-SQL). A continuación se proporciona un ejemplo para restaurar AdventureWorks2019, pero el nombre de la base de datos y la ruta de acceso del archivo de instalación pueden variar en función del entorno. 

Para restaurar AdventureWorks2019 en **Windows**, modifique los valores según corresponda a su entorno y, a continuación, ejecute el siguiente comando de TRANSACT-SQL (T-SQL):


```sql
USE [master]
RESTORE DATABASE [AdventureWorks2019] 
FROM  DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup\AdventureWorks2019.bak' 
WITH  FILE = 1,  NOUNLOAD,  STATS = 5
GO

```

Para restaurar AdventureWorks2019 en **Linux**, cambie la ruta de acceso del sistema de archivos de Windows a Linux y, a continuación, ejecute el siguiente comando de TRANSACT-SQL (T-SQL): 


```sql
USE [master]
RESTORE DATABASE [AdventureWorks2019]
FROM DISK = '/var/opt/mssql/backup/AdventureWorks2019.bak'
WITH MOVE 'AdventureWorks2017' TO '/var/opt/mssql/data/AdventureWorks2019.mdf',
MOVE 'AdventureWorks2017_log' TO '/var/opt/mssql/data/AdventureWorks2019_log.ldf',
FILE = 1,  NOUNLOAD,  STATS = 5
GO
```

# <a name="azure-data-studio"></a>[Azure Data Studio](#tab/data-studio)

Si no está familiarizado con [Azure Data Studio Studio](../azure-data-studio/download-azure-data-studio.md), puede ver [Connect & Query](../azure-data-studio/quickstart-sql-server.md) para comenzar.

Para restaurar la base de datos en Azure Data Studio, siga estos pasos:

1. Descargue el `.bak` archivo adecuado de uno de los vínculos que se proporcionan en la sección [descargar archivos de copia de seguridad](#download-backup-files) .
1. Mueva el `.bak` archivo a la ubicación de copia de seguridad de SQL Server. Esto varía en función de la ubicación de instalación, el nombre de instancia y la versión de SQL Server. Por ejemplo, la ubicación predeterminada de una instancia predeterminada de SQL Server 2019 es:

    `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup`.

1. Abra Azure Data Studio Studio y conéctese a su instancia de SQL Server.
1. Haga clic con el botón derecho en el servidor y seleccione **administrar**.

   :::image type="content" source="media/adventureworks-install-configure/ads-manage.png" alt-text="Captura de pantalla que muestra Azure Data Studio con la opción administrar resaltada y llamada.":::

1. Seleccionar **restauración**

   :::image type="content" source="media/adventureworks-install-configure/ads-restore-database.png" alt-text="Seleccione restaurar en el menú superior para restaurar la base de datos.":::

1. En la pestaña **General** , rellene los valores que aparecen en **origen**.
    1. En **restaurar desde**, seleccione *archivo de copia de seguridad*.
    1. En **ruta de acceso del archivo de copia de seguridad**, seleccione la ubicación en la que almacenó el archivo. bak. 
    
   :::image type="content" source="media/adventureworks-install-configure/ads-source.png" alt-text="Seleccione la ruta de acceso del archivo de copia de seguridad":::
    
    Esto rellena automáticamente el resto de los campos, como la **base de datos**, la base de **datos de destino** y la **restauración en**. 

   :::image type="content" source="media/adventureworks-install-configure/ads-destination-restore-plan.png" alt-text="Una vez que haya elegido una ruta de acceso al archivo de copia de seguridad, el resto de los campos se rellenan automáticamente.":::

1. Seleccione **restaurar** para restaurar la base de datos. 

   :::image type="content" source="media/adventureworks-install-configure/ads-restore.png" alt-text="Una vez que esté listo, seleccione restaurar para restaurar la base de datos.":::

---

## <a name="deploy-to-azure-sql-database"></a>Implementar en Azure SQL Database

Tiene dos opciones para ver los datos de Azure SQL Database de ejemplo. Puede usar un ejemplo al crear una nueva base de datos, o puede implementar una base de datos de SQL Server directamente en Azure mediante SQL Server Management Studio (SSMS).

Para obtener datos de ejemplo de Azure SQL Instancia administrada en su lugar, consulte [restauración de World Wide Importers a SQL instancia administrada](/azure/azure-sql/managed-instance/restore-sample-database-quickstart). 

### <a name="deploy-new-sample-database"></a>Implementar una nueva base de datos de ejemplo

Al crear una nueva base de datos en Azure SQL Database, tiene la opción de crear una base de datos en blanco o una base de datos de ejemplo. 

Siga estos pasos para utilizar una base de datos de ejemplo para crear una nueva base de datos: 

1. Conéctese a su Azure Portal.
1. Seleccione **crear un recurso** en la parte superior izquierda del panel de navegación. 
1. Seleccione **bases de datos** y, a continuación, seleccione **SQL Database**. 
1. Rellene la información solicitada para crear la base de datos. 
1. En la pestaña **configuración adicional** , elija **muestra** como datos existentes en **origen de datos**: 

   :::image type="content" source="media/adventureworks-install-configure/deploy-sample-to-azure.png" alt-text="Elija ejemplo como origen de datos en la pestaña configuración adicional en el Azure Portal al crear el Azure SQL Database":::

1. Seleccione **crear** para crear el nuevo SQL Database, que es la copia restaurada de la base de datos AdventureWorksLT. 


### <a name="deploy-database-from-sql-server"></a>Implementar base de datos desde SQL Server

SQL Server Management Studio proporciona la capacidad de implementar una base de datos directamente en Azure SQL Database. Este método no proporciona actualmente la validación de datos, por lo que está pensada para desarrollo y pruebas, y no debe usarse para producción. 

Para implementar una base de datos de ejemplo desde SQL Server a Azure SQL Database, siga estos pasos:

1. Conéctese al SQL Server en SQL Server Management Studio. 
1. Si todavía no lo ha hecho, [restaure la base de datos de ejemplo en SQL Server](#restore-to-sql-server). 
1. Haga clic con el botón derecho en la base de datos restaurada en **Explorador de objetos**  >  **tareas**  >  **implementar base de datos en Microsoft Azure SQL Database.**... 

   :::image type="content" source="media/adventureworks-install-configure/deploy-db-to-azure.png" alt-text="Elija implementar la base de datos en Microsoft Azure SQL Database haciendo clic con el botón derecho en la base de datos y seleccionando tareas.":::

1. Siga el Asistente para conectarse a Azure SQL Database e implementar la base de datos. 


## <a name="creation-scripts"></a>Scripts de creación

En lugar de restaurar una base de datos, también puede usar scripts para crear las bases de datos de AdventureWorks independientemente de la versión. 

Los scripts siguientes se pueden usar para crear toda la base de datos AdventureWorks: 

- [Zip de scripts OLTP de AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks-oltp-install-script.zip)
- [Código postal de los scripts de AdventureWorks DW](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW-data-warehouse-install-script.zip)

Puede encontrar información adicional sobre el uso de los scripts en [GitHub](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks). 

## <a name="next-steps"></a>Pasos siguientes

Una vez que haya restaurado la base de datos de ejemplo, use los siguientes tutoriales para empezar a trabajar con SQL Server: 


[Tutoriales de SQL Server motor de base de datos](../relational-databases/database-engine-tutorials.md)   
[Conectarse y realizar consultas con SQL Server Management Studio (SSMS)](../ssms/quickstarts/ssms-connect-query-sql-server.md)   
[Conectarse y realizar consultas con Azure Data Studio](../ssms/quickstarts/ssms-connect-query-sql-server.md)