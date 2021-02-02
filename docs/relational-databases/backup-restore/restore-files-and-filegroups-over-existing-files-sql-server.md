---
title: Restauración de archivos (y grupos de archivos) sobre los existentes
description: En este artículo se muestra cómo restaurar archivos y grupos de archivos en archivos existentes en SQL Server mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- restoring files [SQL Server], how-to topics
- restoring files [SQL Server], steps
- file restores [SQL Server], how-to topics
- filegroups [SQL Server], restoring
- restoring filegroups [SQL Server]
- overwriting filegroups
- overwriting files
ms.assetid: 517e07eb-9685-4b06-90af-b1cc496700b7
author: cawrites
ms.author: chadam
ms.openlocfilehash: cf2a0e3637d4fb2f62d26072ed70015d030d2059
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076087"
---
# <a name="restore-files-and-filegroups-over-existing-files-sql-server"></a>Restaurar archivos y grupos de archivos en archivos existentes (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe cómo restaurar archivos y grupos de archivos sobre archivos existentes en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para restaurar archivos y grupos de archivos sobre archivos existentes, utilizando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   El administrador del sistema encargado de restaurar los archivos y grupos de archivos debe ser la única persona que esté utilizando la base de datos.  
  
-   RESTORE no se permite en una transacción explícita o implícita.  
  
-   En el modelo de recuperación completa o el modelo de recuperación optimizado para cargas masivas de registros, para poder restaurar archivos, debe realizar una copia de seguridad del registro de transacciones activo (conocido como el final del registro). Para obtener más información, vea [Realizar copia de seguridad de un registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)).  
  
-   Para restaurar una base de datos cifrada, debe tener acceso al certificado o la clave asimétrica que se usó para cifrarla. La base de datos no se puede restaurar sin el certificado o la clave asimétrica. Como resultado, se debe conservar el certificado que se usa para cifrar la clave de cifrado de base de datos mientras se necesite la copia de seguridad. Para obtener más información, consulte [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Si la base de datos que se va a restaurar no existe, el usuario debe tener permisos CREATE DATABASE para poder ejecutar RESTORE. Si la base de datos existe, los permisos RESTORE corresponden de forma predeterminada a los miembros de los roles fijos de servidor **sysadmin** y **dbcreator** , y al propietario (**dbo**) de la base de datos (para la opción FROM DATABASE_SNAPSHOT, la base de datos siempre existe).  
  
 Los permisos RESTORE se conceden a los roles en los que la información acerca de la pertenencia está siempre disponible para el servidor. Debido a que la pertenencia a un rol fijo de base de datos solo se puede comprobar cuando la base de datos es accesible y no está dañada, lo que no siempre ocurre cuando se ejecuta RESTORE, los miembros del rol fijo de base de datos **db_owner** no tienen permisos RESTORE.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-restore-files-and-filegroups-over-existing-files"></a>Para restaurar archivos y grupos de archivos sobre archivos existentes  
  
1.  En el **Explorador de objetos**, conéctese a una instancia de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], expándala y, a continuación, expanda **Bases de datos**.  
  
2.  Haga clic con el botón derecho en la base de datos que quiera, seleccione **Tareas**, **Restaurar** y, luego, haga clic en **Archivos y grupos de archivos**.  
  
3.  En la página **General** , en el cuadro de lista **A una base de datos** , especifique la base de datos que desea restaurar. Puede especificar una nueva base de datos o elegir una base de datos existente de la lista desplegable. La lista incluye todas las bases de datos del servidor, y excluye las bases de datos del sistema **master** y **tempdb**.  
  
4.  Para especificar el origen y la ubicación de los conjuntos de copias de seguridad que se deben restaurar, haga clic en una de las opciones siguientes:  
  
    -   **Desde base de datos**  
  
         Escriba un nombre de base de datos en el cuadro de lista. La lista contiene solo las bases de datos de las que se ha realizado una copia de seguridad, según el historial de copias de seguridad de **msdb** .  
  
    -   **Desde dispositivo**  
  
         Haga clic en el botón Examinar. En el cuadro de diálogo **Especificar dispositivos de copia de seguridad** , seleccione uno de los tipos de dispositivo enumerados en el cuadro de lista **Tipo de medio de copia de seguridad** . Para seleccionar uno o varios dispositivos del cuadro de lista **Medio para copia de seguridad** , haga clic en **Agregar**.  
  
         Después de agregar los dispositivos que desee al cuadro de lista **Medio de copia de seguridad** , haga clic en **Aceptar** para volver a la página **General** .  
  
5.  En la cuadrícula **Seleccionar los conjuntos de copia de seguridad que se van a restaurar** , seleccione las copias de seguridad que desea restaurar. En esta cuadrícula se muestran las copias de seguridad disponibles en la ubicación especificada. De forma predeterminada, se sugiere un plan de recuperación. Para anular el plan de recuperación sugerido, puede cambiar las selecciones de la cuadrícula. Se anulará automáticamente la selección de aquellas copias de seguridad que dependan de una copia de seguridad cuya selección fue anulada.  
  
    |Encabezado de columna|Valores|  
    |-----------------|------------|  
    |**Restauración**|Las casillas activadas indican los conjuntos de copias de seguridad que se restaurarán.|  
    |**Nombre**|Nombre del conjunto de copia de seguridad.|  
    |**Tipo de archivo**|Especifica el tipo de datos en la copia de seguridad: **Datos**, **Registro** o **Datos de FILESTREAM**. Los datos contenidos en tablas están en archivos de **datos** . Los datos del registro de transacciones están en archivos de **registro** . Los datos de objetos binarios grandes (BLOB) que están almacenados en el sistema de archivos se encuentran en archivos de **datos de FILESTREAM** .|  
    |**Tipo**|Tipo de copia de seguridad realizada: **Completa**, **Diferencial** o **Registro de transacciones**.|  
    |**Server**|Nombre de la instancia del motor de base de datos que ha realizado la operación de copia de seguridad.|  
    |**Nombre lógico de archivo**|Nombre lógico del archivo.|  
    |**Base de datos**|Nombre de la base de datos para la operación de copia de seguridad.|  
    |**Fecha de inicio**|Fecha y hora en la que se inició la operación de copia de seguridad, presentadas en la configuración regional del cliente.|  
    |**Fecha final**|Fecha y hora en la que finalizó la operación de copia de seguridad, presentadas en la configuración regional del cliente.|  
    |**Tamaño**|Tamaño del conjunto de copias de seguridad, en bytes.|  
    |**Nombre de usuario**|Nombre del usuario que realizó la operación de copia de seguridad.|  
  
6.  En el panel **Seleccionar una página** , haga clic en la página **Opciones** .  
  
7.  En el panel **Opciones de restauración** , seleccione **Sobrescribir la base de datos existente (WITH REPLACE)** . La operación de restauración sobrescribe las bases de datos especificadas y sus archivos relacionados, aunque ya exista otra base de datos u otro archivo con el mismo nombre.  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-restore-files-and-filegroups-over-existing-files"></a>Para restaurar archivos y grupos de archivos sobre archivos existentes  
  
1.  Ejecute la instrucción RESTORE DATABASE para restaurar la copia de seguridad de archivos y grupos de archivos; para ello, especifique lo siguiente:  
  
    -   El nombre de la base de datos que se va a restaurar.  
  
    -   El dispositivo de copia de seguridad desde el que se restaurará la copia de seguridad completa de la base de datos.  
  
    -   La cláusula FILE de cada archivo que desee restaurar.  
  
    -   La cláusula FILEGROUP de cada grupo de archivos que desee restaurar.  
  
    -   La opción REPLACE para especificar que cada archivo se puede restaurar sobre archivos existentes que tengan el mismo nombre y ubicación.  
  
        > [!CAUTION]  
        >  Utilice la opción REPLACE con precaución. Para obtener más información, consulte el elemento .  
  
    -   Opción NORECOVERY. Si los archivos no se han modificado desde que se creó la copia de seguridad, especifique la cláusula RECOVERY.  
  
2.  Si los archivos se han modificado después de que se creara la copia de seguridad, ejecute la instrucción RESTORE LOG para aplicar la copia de seguridad del registro de transacciones y especifique:  
  
    -   El nombre de la base de datos a la que se aplicará el registro de transacciones.  
  
    -   El dispositivo de copia de seguridad desde el que se restaurará la copia de seguridad del registro de transacciones.  
  
    -   La cláusula NORECOVERY, si hay otra copia de seguridad del registro de transacciones que se deba aplicar después de la actual; de lo contrario, especifique la cláusula RECOVERY.  
  
         Las copias de seguridad del registro de transacciones, si se aplican, deben cubrir el período de tiempo en el que se hizo la copia de seguridad de los archivos y grupos de archivos.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Ejemplo (Transact-SQL)  
 En el siguiente ejemplo se restauran los archivos y grupos de archivos de la base de datos `MyNwind` y se reemplaza cualquier archivo existente del mismo nombre. También se aplicarán dos registros de transacciones para restaurar la base de datos a la hora actual.  
  
```sql  
USE master;  
GO  
-- Restore the files and filesgroups for MyNwind.  
RESTORE DATABASE MyNwind  
   FILE = 'MyNwind_data_1',  
   FILEGROUP = 'new_customers',  
   FILE = 'MyNwind_data_2',  
   FILEGROUP = 'first_qtr_sales'  
   FROM MyNwind_1  
   WITH NORECOVERY,  
   REPLACE;  
GO  
-- Apply the first transaction log backup.  
RESTORE LOG MyNwind  
   FROM MyNwind_log1  
   WITH NORECOVERY;  
GO  
-- Apply the last transaction log backup.  
RESTORE LOG MyNwind  
   FROM MyNwind_log2  
   WITH RECOVERY;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Restaurar archivos y grupos de archivos &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)   
 [Copiar bases de datos con Copias de seguridad y restauración](../../relational-databases/databases/copy-databases-with-backup-and-restore.md)  
  
  
