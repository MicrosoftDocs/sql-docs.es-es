---
description: DBCC CLONEDATABASE (Transact-SQL)
title: DBCC CLONEDATABASE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/23/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CLONEDATABASE
- CLONE DATABASE
- DBCC_CLONEDATABASE_TSQL
- DBCC CLONEDATABASE
- DBCC CLONE DATABASE
- CLONEDATABASE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database cloning [SQL Server]
- cloning databases
- clone databases
- cloning database
- clone database
- copying databases
- copy databases
- copying database
- copy database
- NO_STATISTICS option
- NO_QUERYSTORE option
- VERIFY_CLONEDB option
- BACKUP_CLONEDB option
- database copying [SQL Server]
- database cloning [SQL Server]
- DBCC CLONEDATABASE statement
ms.assetid: ''
author: bluefooted
ms.author: pamela
manager: amitban
ms.openlocfilehash: e0ddfa333535c48d3338eb26a078a67bbf7079d8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340657"
---
# <a name="dbcc-clonedatabase-transact-sql"></a>DBCC CLONEDATABASE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Genera un clon de solo esquema de una base de datos mediante DBCC CLONEDATABASE para investigar problemas de rendimiento relacionados con el optimizador de consultas.

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DBCC CLONEDATABASE   
(  
    source_database_name
    ,  target_database_name
)
    [ WITH { [ NO_STATISTICS ] [ , NO_QUERYSTORE ] [ , VERIFY_CLONEDB | SERVICEBROKER ] [ , BACKUP_CLONEDB ] } ]     
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*source_database_name*  
El nombre de la base de datos que se va a copiar. 
  
*target_database_name*  
El nombre de la base de datos en la que se copiará la base de datos de origen. DBCC CLONEDATABASE creará esta base de datos, que no debería existir. 
  
NO_STATISTICS  
Especifica si las estadísticas de tabla o índice se deben excluir de la clonación. Si no se especifica esta opción, se incluirán automáticamente estadísticas de tabla o de índice. Esta opción está disponible a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3 y [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1.

NO_QUERYSTORE<br>
Especifica si se deben excluir del clon los datos del almacén de consultas. Si no se especifica esta opción, los datos del almacén de consultas se copiarán en el clon si el almacén de consultas está habilitado en la base de datos de origen. Esta opción está disponible a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1.

VERIFY_CLONEDB  
Comprueba la coherencia de la base de datos nueva.  Esta opción es necesaria si la base de datos clonada está pensada para usarse en producción.  La acción de habilitar VERIFY_CLONEDB también deshabilita la recopilación de estadísticas y del almacén de consultas, por lo que equivale a ejecutar WITH VERIFY_CLONEDB, NO_STATISTICS y NO_QUERYSTORE.  Esta opción está disponible a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP3, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU8.

> [!NOTE]  
> El comando siguiente se puede usar para confirmar que la base de datos clonada está preparada para usarse en producción: <br/>`SELECT DATABASEPROPERTYEX('clone_database_name', 'IsVerifiedClone')`


SERVICEBROKER<br>
Especifica si los catálogos del sistema relacionados con Service Broker deben incluirse en el clon.  No se puede usar la opción SERVICEBROKER en combinación con VERIFY_CLONEDB.  Esta opción está disponible a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP3, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU8.

BACKUP_CLONEDB  
Crea y comprueba una copia de seguridad de la base de datos clonada.  Si se usa en combinación con VERIFY_CLONEDB, la base de datos clonada se comprueba antes de que se realice la copia de seguridad.  Esta opción está disponible a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP3, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU8.
  
## <a name="remarks"></a>Observaciones
DBCC CLONEDATABASE lleva a cabo las siguientes validaciones. Se produce un error en el comando si alguna de las validaciones no se efectúa correctamente.
- La base de datos de origen debe ser una base de datos de usuario. No se permite clonar bases de datos del sistema (master, model, msdb, tempdb, bases de datos de distribución, etc.).
- La base de datos de origen debe ser legible o estar en línea.
- No puede existir una base de datos que use el mismo nombre que la base de datos clonada.
- El comando no se encuentra en una transacción de usuario.

Si todas las validaciones se efectúan correctamente, la clonación de la base de datos de origen se lleva a cabo mediante las siguientes operaciones:
- Crea una base de datos de destino que usa el mismo diseño de archivo como origen, pero tiene tamaños de archivo predeterminados de la base de datos modelo.
- Crea una instantánea interna de la base de datos de origen.
- Copia los metadatos del sistema de la base de datos de origen a la base de datos de destino.
- Copia todos los esquemas de todos los objetos de la base de datos de origen a la base de datos de destino.
- Copia las estadísticas de todos los índices de la base de datos de origen a la base de datos de destino.

> [!NOTE]  
> La nueva base de datos generada a partir de DBCC CLONEDATABASE sirve principalmente para fines de diagnóstico y de solución de problemas.  Para que la base de datos clonada se pueda usar como base de datos de producción, se debe usar la opción VERIFY_CLONEDB.

Todos los archivos de la base de datos de destino heredarán la configuración de tamaño y crecimiento de la base de datos modelo. Los nombres de archivo de la base de datos de destino seguirán la convención de números source_file_name _underscore_random. Si el nombre del archivo generado ya existe en la carpeta de destino, se producirá un error en DBCC CLONEDATABASE.

DBCC CLONEDATABASE no admite la creación de un clon si hay algún objeto de usuario (tablas, índices, esquemas, roles, etc.) que se ha creado en la base de datos modelo. Si hay objetos de usuario en la base de datos modelo, se produce un error en el clon de la base de datos con el siguiente mensaje de error:

```
Msg 2601, Level 14, State 1, Line 1
Cannot insert duplicate key row in object <system table> with unique index 'index name'. The duplicate key value is <key value>   
```

> [!IMPORTANT]
> Si tiene índices de almacén de columnas, vea [Consideraciones al optimizar las consultas con índices de almacén de columnas en las bases de datos clonadas](https://techcommunity.microsoft.com/t5/SQL-Server/Considerations-when-tuning-your-queries-with-columnstore-indexes/ba-p/385294) para actualizar las estadísticas de los índices de almacén de columnas antes de ejecutar el comando **DBCC CLONEDATABASE**.  A partir de SQL Server 2019, los pasos manuales descritos en el artículo anterior ya no serán necesarios, ya que el comando **DBCC CLONEDATABASE** recopila esta información automáticamente.

<a name="ctp23"></a>

## <a name="stats-blob-for-columnstore-indexes"></a>Blob de estadísticas para los índices de almacén de columnas

En [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], `DBCC CLONEDATABASE` captura automáticamente los blobs de estadísticas de índices de almacén de columnas, por lo que no es necesario realizar ningún paso manual.`DBCC CLONEDATABASE` crea una copia de solo esquema de una base de datos que incluye todos los elementos necesarios para solucionar problemas de rendimiento de consultas sin copiar los datos. En versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el comando no copió las estadísticas necesarias para solucionar con precisión los problemas de las consultas del índice de almacén de columnas y se tuvieron que realizar pasos manuales para capturar esta información.

Para obtener información relacionada con la seguridad de los datos en bases de datos clonadas, vea [Understanding data security in cloned databases](https://techcommunity.microsoft.com/t5/SQL-Server/Understanding-data-security-in-cloned-databases-created-using/ba-p/385287) (Descripción de la seguridad de los datos en bases de datos clonadas).

## <a name="internal-database-snapshot"></a>Instantánea de base de datos interna
DBCC CLONEDATABASE usa una instantánea de base de datos interna de la base de datos de origen para la coherencia transaccional que se necesita para realizar la copia. Con esta instantánea se evitan problemas de bloqueo y de simultaneidad cuando se ejecutan estos comandos. Si no se puede crear una instantánea, se producirá un error en DBCC CLONEDATABASE. 

Durante los siguientes pasos del proceso de copia se mantienen bloqueos de nivel de base de datos:
- Validar la base de datos de origen
- Obtener el bloqueo S para la base de datos de origen
- Crear una instantánea de la base de datos de origen
- Crear una base de datos clonada (una base de datos vacía heredada de la base de datos modelo)
- Obtener el bloqueo X para la base de datos clonada
- Copiar los metadatos en la base de datos clonada
- Liberar todos los bloqueos de base de datos

Tan pronto como el comando haya terminado de ejecutarse, se quitará la instantánea interna. Las opciones TRUSTWORTHY y DB_CHAINING están desactivadas en una base de datos clonada. 

## <a name="supported-objects"></a>Objetos admitidos
Solo se pueden clonar los siguientes objetos en la base de datos de destino. Los objetos cifrados se clonan, pero no se pueden usar en la base de datos clonada. Los objetos que no figuran en la siguiente sección no se admiten en el clon: 
- APPLICATION ROLE
- AVAILABILITY GROUP
- COLUMNSTORE INDEX
- CDB
- CDC
- CLR (a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 y versiones posteriores)
- DATABASE PROPERTIES
- DEFAULT
- FILES AND FILEGROUPS
- Texto completo (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 CU2)
- FUNCTION
- INDEX
- LOGIN
- PARTITION FUNCTION
- PARTITION SCHEME
- PROCEDURE   
> [!NOTE]   
> Los procedimientos de [!INCLUDE[tsql](../../includes/tsql-md.md)] se admiten en todas las versiones a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2. Los procedimientos de CLR se admiten a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3. A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 se admiten los procedimientos compilados de forma nativa.  

- QUERY STORE (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1)   
> [!NOTE]   
> Los datos de almacén de consultas solo se copian si están habilitados en la base de datos de origen. Para copiar las estadísticas en tiempo de ejecución más recientes como parte del almacén de consultas, ejecute sp_query_store_flush_db para vaciar las estadísticas en tiempo de ejecución en el almacén de consultas antes de ejecutar DBCC CLONEDATABASE.  

- ROLE
- RULE
- SCHEMA
- SEQUENCE
- SPATIAL INDEX
- STATISTICS
- SYNONYM
- TABLE
- MEMORY OPTIMIZED TABLES (solo en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 y en versiones posteriores).
- FILESTREAM AND FILETABLE OBJECTS (a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 y versiones posteriores). 
- TRIGGER
- TYPE
- UPGRADED DB
- USER
- VIEW
- XML INDEX
- XML SCHEMA COLLECTION  

## <a name="permissions"></a>Permisos  
Requiere la pertenencia al rol fijo de servidor **sysadmin** .

## <a name="error-log-messages"></a>Mensajes de registro de error
Los siguientes mensajes son un ejemplo de los mensajes registrados en el registro de errores durante el proceso de clonación:

```
2018-03-26 15:33:56.05 spid53 Database cloning for 'sourcedb' has started with target as 'sourcedb_clone'.

2018-03-26 15:33:56.46 spid53 Starting up database 'sourcedb_clone'.

2018-03-26 15:33:57.80 spid53 Setting database option TRUSTWORTHY to OFF for database 'sourcedb_clone'.

2018-03-26 15:33:57.80 spid53 Setting database option DB_CHAINING to OFF for database 'sourcedb_clone'.

2018-03-26 15:33:57.88 spid53 Starting up database 'sourcedb_clone'.

2018-03-26 15:33:57.91 spid53 Database 'sourcedb_clone' is a cloned database. A cloned database should be used for diagnostic purposes only and is not supported for use in a production environment.

2018-03-26 15:33:57.92 spid53 Database cloning for 'sourcedb' has finished. Cloned database is 'sourcedb_clone'.
```

## <a name="database-properties"></a>Propiedades de la base de datos
`DATABASEPROPERTYEX('dbname', 'IsClone')` devolverá 1 si la base de datos se generó mediante DBCC CLONEDATABASE.

`DATABASEPROPERTYEX('dbname', 'IsVerifiedClone')` devolverá 1 si la base de datos se ha comprobado correctamente con WITH VERIFY_CLONEDB.

## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-clone-of-a-database-that-includes-schema-statistics-and-query-store"></a>A. Crear un clon de una base de datos que incluye un esquema, estadísticas y un almacén de consultas 
En el ejemplo siguiente se crea un clon de la base de datos AdventureWorks que incluye datos de esquema, de estadísticas y de almacén de consultas ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 y versiones posteriores)

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone);    
GO 
```  
  
### <a name="b-creating-a-schema-only-clone-of-a-database-without-statistics"></a>B. Crear un clon de solo esquema de una base de datos sin estadísticas 
En el ejemplo siguiente se crea un clon de la base de datos AdventureWorks que no incluye estadísticas ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3 y versiones posteriores)

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH NO_STATISTICS;    
GO 
```  

### <a name="c-creating-a-schema-only-clone-of-a-database-without-statistics-and-query-store"></a>C. Crear un clon de solo esquema de una base de datos sin estadísticas y sin almacén de consultas 
En el ejemplo siguiente se crea un clon de la base de datos AdventureWorks que no incluye datos de estadísticas ni de almacén de consultas ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 y versiones posteriores)

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH NO_STATISTICS, NO_QUERYSTORE;    
GO 
```  

### <a name="d-creating-a-clone-of-a-database-that-is-verified-for-production-use"></a>D. Crear un clon de una base de datos que se comprueba para su uso en producción
En el ejemplo siguiente se crea un clon de solo esquema de la base de datos AdventureWorks sin datos de estadísticas ni de almacén de consultas que se comprueba para su uso como base de datos de producción ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y versiones posteriores).

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH VERIFY_CLONEDB;    
GO 
```  
  
### <a name="e-creating-a-clone-of-a-database-that-is-verified-for-production-use-that-includes-a-backup-of-the-cloned-database"></a>E. Crear un clon de una base de datos que se comprueba para su uso en producción que incluye una copia de seguridad de la base de datos clonada
En el ejemplo siguiente se crea un clon de solo esquema de la base de datos AdventureWorks sin datos de estadísticas ni de almacén de consultas que se comprueba para su uso como base de datos de producción.  También se creará una copia de seguridad comprobada de la base de datos clonada ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y versiones posteriores).

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH VERIFY_CLONEDB, BACKUP_CLONEDB;    
GO 
```

## <a name="see-also"></a>Consulte también
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)    
[Cómo generar un script de los metadatos de base de datos necesarios para crear una base de datos de solo estadísticas en SQL Server](https://support.microsoft.com/help/914288)   

