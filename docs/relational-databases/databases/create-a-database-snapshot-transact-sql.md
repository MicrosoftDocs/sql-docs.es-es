---
title: Creación de una instantánea de base de datos (Transact-SQL) | Microsoft Docs
description: Obtenga información sobre cómo crear una instantánea de base de datos de SQL Server mediante Transact-SQL. Obtenga información sobre los requisitos previos y los procedimientos recomendados para crear instantáneas.
ms.custom: ''
ms.date: 08/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- database snapshots [SQL Server], creating
ms.assetid: 187fbba3-c555-4030-9bdf-0f01994c5230
author: stevestein
ms.author: sstein
ms.openlocfilehash: d0418f33afd634cc5ec4d9d7b7a887e6bdae6fc9
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813265"
---
# <a name="create-a-database-snapshot-transact-sql"></a>Crear una instantánea de base de datos (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  El único modo de crear una instantánea de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consiste en usar [!INCLUDE[tsql](../../includes/tsql-md.md)]. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] no admite la creación de instantáneas de base de datos.  
  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
 La base de datos de origen, que puede usar cualquier modelo de recuperación, debe cumplir los siguientes requisitos previos:  
  
-   La instancia del servidor debe ejecutarse en una edición de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que admita instantáneas de bases de datos. Para obtener más información sobre la compatibilidad con la creación de instantáneas de base de datos en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
-   La base de datos de origen debe estar en línea, a menos que sea una base de datos reflejada dentro de una sesión de creación de reflejo de la base de datos.  
  
-   Para crear una instantánea de base de datos en una base de datos reflejada, la base de datos debe hallarse en [estado de reflejo](../../database-engine/database-mirroring/mirroring-states-sql-server.md)sincronizado.  
  
-   La base de datos de origen no se puede configurar como una base de datos compartida escalable.  

::: moniker range="=sql-server-2016||=sql-server-2017"

- La base de datos de origen no debe contener un grupo de archivos MEMORY_OPTIMIZED_DATA.

:::moniker-end

> [!IMPORTANT]
> Para obtener información sobre otras consideraciones importantes, vea [Instantáneas de base de datos &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md).  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
 En esta sección se describen los procedimientos recomendados siguientes:  
  
-   [Procedimiento recomendado: Asignar nombres a instantáneas de base de datos](#Naming)  
  
-   [Procedimiento recomendado: Limitar el número de instantáneas de base de datos](#Limiting_Number)  
  
-   [Procedimiento recomendado: Conexiones de cliente con una instantánea de base de datos](#Client_Connections)  
  
####  <a name="best-practice-naming-database-snapshots"></a><a name="Naming"></a> Procedimiento recomendado: Asignar nombres a instantáneas de base de datos  
 Antes de crear instantáneas, es importante pensar cómo asignarles un nombre. Cada instantánea de base de datos necesita un nombre de base de datos único. Para facilitar la administración, el nombre de una instantánea puede incorporar información que identifique la base de datos, por ejemplo:  
  
-   Nombre de la base de datos de origen.  
  
-   Una indicación de que el nuevo nombre es para una instantánea.  
  
-   La fecha y hora de creación de la instantánea, un número de secuencia o cualquier otra información, por ejemplo, la hora del día, para distinguir instantáneas secuenciales en una base de datos dada.  
  
 Por ejemplo, piense en una serie de instantáneas de base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . Se crean tres instantáneas diarias a intervalos de 6 horas entre las 06:00 y las 18:00, tomando como base un reloj de 24 horas. Cada instantánea diaria se conserva 24 horas antes de que se quite y sea reemplazada por una nueva instantánea con el mismo nombre. Recuerde que cada nombre de instantánea indica la hora, pero no el día:  
  
```  
AdventureWorks_snapshot_0600  
AdventureWorks_snapshot_1200  
AdventureWorks_snapshot_1800  
```  
  
 Como alternativa, si la hora de creación de estas instantáneas diarias varía cada día, es posible que sea preferible disponer de una convención de nomenclatura menos precisa, por ejemplo:  
  
```  
AdventureWorks_snapshot_morning  
AdventureWorks_snapshot_noon  
AdventureWorks_snapshot_evening  
```  
  
#### <a name="best-practice-limiting-the-number-of-database-snapshots"></a><a name="Limiting_Number"></a> Procedimiento recomendado: Limitar el número de instantáneas de base de datos  
 La creación de una serie de instantáneas a lo largo del tiempo permite capturar instantáneas secuenciales de la base de datos de origen. Cada instantánea se conserva hasta que se quite de manera explícita. Las instantáneas siguen creciendo a medida que se actualizan las páginas originales, por lo que seguramente querrá ahorrar espacio en el disco eliminando una instantánea más antigua después de crear una nueva instantánea.  
  

**Nota:** Para volver a una instantánea de base de datos, debe eliminar cualquier otra instantánea de esa base de datos.  
  
####  <a name="best-practice-client-connections-to-a-database-snapshot"></a><a name="Client_Connections"></a> Procedimiento recomendado: Conexiones de cliente con una instantánea de base de datos  
 Para usar una instantánea de base de datos, los clientes deben saber dónde encontrarla. Los usuarios pueden leer de una instantánea de base de datos mientras se crea o elimina otra. Sin embargo, si sustituye una nueva instantánea por otra ya existente, debe redirigir a los clientes a la nueva instantánea. Los usuarios pueden conectarse manualmente a una instantánea de base de datos mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Sin embargo, para admitir un entorno de producción, debe crear una solución programática que dirija de un modo transparente a los clientes de escritura de informes a la instantánea de base de datos más reciente de la base de datos.  
  

  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Todos los usuarios que pueden crear una base de datos pueden crear una instantánea de base de datos; sin embargo, para crear una instantánea de una base de datos reflejada, es necesario ser miembro del rol fijo de servidor **sysadmin** .  
  
##  <a name="how-to-create-a-database-snapshot-using-transact-sql"></a><a name="TsqlProcedure"></a> Cómo crear una instantánea de base de datos (con Transact-SQL)  
 **Para crear una instantánea de base de datos**  
  
>  Para obtener un ejemplo de este procedimiento, vea [Ejemplos (Transact-SQL)](#TsqlExample), más adelante en esta sección.  
  
1.  Basándose en el tamaño actual de la base de datos de origen, asegúrese de que tiene suficiente espacio en disco para incluir la instantánea de base de datos. El tamaño máximo de una instantánea de base de datos es el tamaño de la base de datos de origen en el momento de la creación de la instantánea. Para obtener más información, vea [Ver el tamaño del archivo disperso de una instantánea de base de datos &#40;Transact-SQL&#41;](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md).  
  
2.  Emita una instrucción CREATE DATABASE en los archivos que usen la cláusula AS SNAPSHOT OF. Para crear una instantánea, se debe especificar el nombre lógico de cada archivo de la base de datos de origen. La sintaxis es la siguiente:  

     CREATE DATABASE *nombre_de_instantánea_de_base_de_datos*  
  
     ACTIVAR  
  
     (  
  
     NAME =*nombre_de_archivo_lógico*,  
  
     FILENAME ='*nombre_de_archivo_de_sistema_operativo*'  
  
     ) [ ,...*n* ]  
  
     AS SNAPSHOT OF *nombre_de_base_de_datos_de_origen*  
  
     [;]  
  
     Donde *nombre_base_de_datos**_origen* es la base de datos de origen, *nombre_de_archivo_lógico* es el nombre lógico que se usa en SQL Server cuando se hace referencia al archivo, *nombre_de_archivo_de_sistema_operativo* es la ruta de acceso y el nombre de archivo que usa el sistema operativo cuando se crea el archivo y *nombre_de_instantánea_de_base_de_datos* es el nombre de la instantánea a la que quiere revertir la base de datos. Para obtener una descripción completa de esta sintaxis, vea [CREATE DATABASE &#40;Transact-SQL de SQL Server&#41;](../../t-sql/statements/create-database-transact-sql.md).  
  
    > [!NOTE]  
    >  Cuando se crea una instantánea de base de datos, no se permite la presencia de archivos de registro, archivos sin conexión, archivos de restauración e inactivos en la instrucción CREATE DATABASE.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Ejemplos (Transact-SQL)  
  
> [!NOTE]  
>  La extensión `.ss` que se usa en los ejemplos es arbitraria.  
  
 En esta sección se incluyen los ejemplos siguientes:  
  
-   A. [Crear una instantánea de la base datos AdventureWorks](#Creating_on_AW)  
  
-   B. [Crear una instantánea de la base datos Sales](#Creating_on_Sales)
  
####  <a name="a-creating-a-snapshot-on-the-adventureworks-database"></a><a name="Creating_on_AW"></a> A. Crear una instantánea de la base datos AdventureWorks  
 En este ejemplo se crea una instantánea de base datos `AdventureWorks` . El nombre de la instantánea, `AdventureWorks_dbss_1800`, y el nombre de archivo de su archivo disperso, `AdventureWorks_data_1800.ss`, indican la hora de creación: 6 P.M (1800 horas).  
  
```  
CREATE DATABASE AdventureWorks_dbss1800 ON  
( NAME = AdventureWorks, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks_data_1800.ss' )  
AS SNAPSHOT OF AdventureWorks;  
GO  
```  
  
####  <a name="b-creating-a-snapshot-on-the-sales-database"></a><a name="Creating_on_Sales"></a> B. Crear una instantánea de la base datos Sales  
 En este ejemplo se crea una instantánea de base datos, `sales_snapshot1200`, en la base de datos `Sales` . Esta base de datos se creó en el ejemplo "Crear una base de datos con grupos de archivos", en [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md).  
  
```  
--Creating sales_snapshot1200 as snapshot of the  
--Sales database:  
CREATE DATABASE sales_snapshot1200 ON  
( NAME = SPri1_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\data\SPri1dat_1200.ss'),  
( NAME = SPri2_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\data\SPri2dt_1200.ss'),  
( NAME = SGrp1Fi1_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\mssql\data\SG1Fi1dt_1200.ss'),  
( NAME = SGrp1Fi2_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\data\SG1Fi2dt_1200.ss'),  
( NAME = SGrp2Fi1_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\data\SG2Fi1dt_1200.ss'),  
( NAME = SGrp2Fi2_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\data\SG2Fi2dt_1200.ss')  
AS SNAPSHOT OF Sales;  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Ver una instantánea de base de datos &#40;SQL Server&#41;](../../relational-databases/databases/view-a-database-snapshot-sql-server.md)  
  
-   [Revertir una base de datos a una instantánea de base de datos](../../relational-databases/databases/revert-a-database-to-a-database-snapshot.md)  
  
-   [Eliminar una instantánea de base de datos &#40;Transact-SQL&#41;](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md)  
  
## <a name="see-also"></a>Consulte también  
 [CREATE DATABASE &#40;Transact-SQL de SQL Server&#41;](../../t-sql/statements/create-database-transact-sql.md)   
 [Instantáneas de bases de datos &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md)  
  
