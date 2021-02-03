---
description: DROP DATABASE (Transact-SQL)
title: DROP DATABASE (Transact-SQL)
ms.custom: ''
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP DATABASE
- DROP_DATABASE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- snapshots [SQL Server database snapshots], deleting
- removing databases
- database snapshots [SQL Server], removing
- deleting databases
- dropping databases
- databases [SQL Server], dropping
- DROP DATABASE statement
- database removal [SQL Server], DROP DATABASE statement
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ffdf0615671d2f3e019bccde83159bdb75eddcdd
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236276"
---
# <a name="drop-database-transact-sql"></a>DROP DATABASE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Quita una o varias bases de datos de usuario o instantáneas de base de datos de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintaxis

```syntaxsql
-- SQL Server Syntax
DROP DATABASE [ IF EXISTS ] { database_name | database_snapshot_name } [ ,...n ] [;]
```

```syntaxsql
-- Azure SQL Database, Azure Synapse Analytics and Analytics Platform System Syntax
DROP DATABASE database_name [;]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

 *IF EXISTS*  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).

Quita la base de datos condicionalmente solo si ya existe.

*database_name* Especifica el nombre de la base de datos que se va a quitar. Para mostrar una lista de bases de datos, use la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

*database_snapshot_name*
**Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores

Especifica el nombre de la instantánea de base de datos que se va a quitar.

## <a name="general-remarks"></a>Notas generales

Una base de datos se puede quitar sea cual sea su estado (sin conexión, solo lectura, sospechosa, etc.). Para ver el estado actual de una base de datos, use la vista de catálogo **sys.databases**.

Una base de datos que se ha quitado solo puede volver a crearse si se restaura una copia de seguridad. No es posible realizar copias de seguridad de instantáneas de la base de datos, por lo que éstas no se pueden restaurar.

Cuando se quita una base de datos, debe realizarse una copia de seguridad de la [base de datos maestra](../../relational-databases/databases/master-database.md).

Al quitar una base de datos, se elimina la base de datos de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], así como los archivos de disco físico que usa. Si la base de datos o alguno de sus archivos están sin conexión cuando se quita, no se eliminan los archivos de disco. Estos archivos se pueden eliminar manualmente en el Explorador de Windows. Para quitar una base de datos del servidor actual sin eliminar los archivos del sistema de archivos, use [sp_detach_db](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md).

> [!WARNING]
> La acción de quitar una base de datos que tenga asociadas copias de seguridad de FILE_SNAPSHOT se llevará a cabo correctamente, pero no se eliminarán los archivos de base de datos que tengan instantáneas asociadas para evitar que se invaliden las copias de seguridad que hagan referencia a dichos archivos de base de datos. El archivo se truncará, pero no se eliminará físicamente con el fin de mantener intactas las copias de seguridad de FILE_SNAPSHOT. Para más información, consulte [Copia de seguridad y restauración de SQL Server con el servicio Microsoft Azure Blob Storage](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md). **Se aplica a**: de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] a la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level).

### [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Cuando se quita una instantánea de base de datos, se elimina la instantánea de base de datos de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], así como los archivos físicos dispersos del sistema de archivos NTFS que emplea. Para obtener más información sobre cómo las instantáneas de base de datos usan archivos dispersos, vea [Instantáneas de base de datos](../../relational-databases/databases/database-snapshots-sql-server.md). Al quitar una instantánea de la base de datos se borra la caché del plan para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Al borrar la memoria caché de planes, se provoca una nueva compilación de todos los planes de ejecución posteriores y puede ocasionar una disminución repentina y temporal del rendimiento de las consultas. Para cada almacén de caché borrado de la caché de planes, el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contendrá el siguiente mensaje informativo: "[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha detectado %d instancias de vaciado del almacén de caché "%s" (parte de la memoria caché de planes) debido a determinadas operaciones de mantenimiento de base de datos o reconfiguración". Este mensaje se registra cada cinco minutos siempre que se vacíe la memoria caché dentro de ese intervalo de tiempo.

## <a name="interoperability"></a>Interoperabilidad

### <a name="sql-server"></a>SQL Server

Para quitar una base de datos publicada para la replicación transaccional, o suscrita o publicada para la replicación de mezcla, primero es necesario quitar la replicación de la base de datos. Si se daña una base de datos o no se puede quitar la replicación primero, o ambas cosas, la mayoría de las veces todavía se puede quitar la base de datos utilizando ALTER DATABASE para definirla como sin conexión y, después, quitarla.

Si la base de datos participa en el trasvase de registros, quite el trasvase de registros antes de quitar la base de datos. Para obtener más información, consulte [Acerca del trasvase de registros](../../database-engine/log-shipping/about-log-shipping-sql-server.md).

## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones

Las [bases de datos del sistema](../../relational-databases/databases/system-databases.md) no se pueden quitar.

La instrucción DROP DATABASE debe ejecutarse en modo de confirmación automática y no se permite en una transacción explícita o implícita. El modo de confirmación automática es el modo de administración de transacciones predeterminado.

No se puede quitar una base de datos que se está utilizando actualmente. Es decir, que un usuario la tenga abierta para lectura o escritura. Una manera para quitar usuarios de la base de datos consiste en usar ALTER DATABASE para establecer la base de datos en SINGLE_USER.

> [!WARNING]
> Este no se trata de un enfoque de prueba de errores, ya que la primera conexión consecutiva efectuada por cualquier subproceso recibirá el subproceso SINGLE_USER, lo cual provocará un error de conexión. SQL Server no proporciona ningún método integrado para quitar bases de datos que se están cargando.

### [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Deben quitarse las instantáneas de una base de datos para poder quitar esa base de datos.

La acción de quitar una base de datos habilitada para Stretch Database no elimina los datos remotos. Si quiere eliminar los datos remotos, tendrá que hacerlo manualmente.

### [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Debe estar conectado a la base de datos maestra para quitar una base de datos.

 La instrucción DROP DATABASE debe ser la única instrucción en un lote SQL y solo puede quitar una base de datos cada vez.

### [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]

Debe estar conectado a la base de datos maestra para quitar una base de datos.

La instrucción DROP DATABASE debe ser la única instrucción en un lote SQL y solo puede quitar una base de datos cada vez.

## <a name="permissions"></a>Permisos

### [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Requiere el permiso **CONTROL** en la base de datos, el permiso **ALTER ANY DATABASE** o la pertenencia al rol fijo de base de datos **db_owner**.

### [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Solo el inicio de sesión principal de nivel de servidor (creado por el proceso de aprovisionamiento) o los miembros del rol de base de datos **dbmanager** pueden quitar una base de datos.

### [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Requiere el permiso **CONTROL** en la base de datos, el permiso **ALTER ANY DATABASE** o la pertenencia al rol fijo de base de datos **db_owner**.

## <a name="examples"></a>Ejemplos

### <a name="a-dropping-a-single-database"></a>A. Quitar una sola base de datos

En el ejemplo siguiente se quita la base de datos `Sales`.

```sql
DROP DATABASE Sales;
```

### <a name="b-dropping-multiple-databases"></a>B. Quitar varias bases de datos

**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.

En el ejemplo siguiente se quita cada una de las bases de datos enumeradas.

```sql
DROP DATABASE Sales, NewSales;
```

### <a name="c-dropping-a-database-snapshot"></a>C. Quitar una instantánea de la base de datos

**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.

En el ejemplo siguiente se quita una instantánea de base de datos, denominada `sales_snapshot0600`, sin que la base de datos de origen se vea afectada.

```sql
DROP DATABASE sales_snapshot0600;
```

## <a name="see-also"></a>Consulte también

- [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)