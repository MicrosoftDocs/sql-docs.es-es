---
title: Estadísticas para las tablas con optimización para memoria | Microsoft Docs
description: Obtenga información sobre cómo el optimizador de consultas utiliza las estadísticas de las columnas de las tablas optimizadas para memoria para crear planes de consulta que mejoren el rendimiento de las consultas para OLTP en memoria.
ms.custom: ''
ms.date: 10/23/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: e644766d-1d1c-43d7-83ff-8ccfe4f3af9f
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 119d3702dd315e424c56a3e1431b42a79ca636d4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485167"
---
# <a name="statistics-for-memory-optimized-tables"></a>Estadísticas para las tablas con optimización para memoria
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  El optimizador de consultas utiliza las estadísticas de las columnas para crear planes de consulta que mejoren el rendimiento de las consultas. Las estadísticas se recopilan de las tablas de la base de datos y se almacenan en los metadatos de la base de datos.  
  
 Las estadísticas se crean automáticamente, pero también se pueden crear manualmente. Por ejemplo, para las columnas de clave de índice, las estadísticas se crean automáticamente cuando se crea el índice. Para obtener más información acerca de cómo crear estadísticas, vea [Statistics](../../relational-databases/statistics/statistics.md).  
  
 Normalmente, los datos de tabla cambian a medida que se insertan, se actualizan y se eliminan filas. Esto significa que las estadísticas deben actualizarse periódicamente. De forma predeterminada, las estadísticas de tablas se actualizan automáticamente cuando el optimizador de consultas determina que podrían estar obsoletas.  
  
 Consideraciones sobre las estadísticas de tablas optimizadas para memoria:  
  
-   A partir de SQL Server 2016 y de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], se admite la actualización automática de estadísticas de tablas optimizadas para memoria, si se usa un nivel de compatibilidad de base de datos de al menos 130. Vea [Nivel de compatibilidad de ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md). Si una base de datos tiene tablas creadas anteriormente con un nivel de compatibilidad inferior, las estadísticas se tendrán que actualizar manualmente una vez para, así, poder actualizarlas automáticamente desde ese instante.
  
-   En el caso de los procedimientos almacenados compilados de forma nativa, los planes de ejecución de las consultas del procedimiento se optimizan cuando se compila el procedimiento, algo que tiene lugar al crearlo. No se vuelven a compilar automáticamente cuando se actualizan las estadísticas. Por lo tanto, las tablas deben contener un conjunto de datos representativo para que los procedimientos puedan crearse.  
  
-   Los procedimientos almacenados compilados de forma nativa se pueden compilar manualmente con [sp_recompile (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md)y se vuelven a compilar automáticamente si la base de datos se desconecta y se vuelve a conectar, o bien cuando hay un reinicio del servidor o una conmutación por error de la base de datos.  
  
## <a name="enabling-automatic-update-of-statistics-in-existing-tables"></a>Habilitar la actualización automática de estadísticas de las tablas existentes

Cuando se crean tablas en una base de datos que tiene un nivel de compatibilidad de, como mínimo, 130, la actualización automática de estadísticas estará habilitada para todas las estadísticas de la tabla y no será necesario realizar ninguna acción.

Si una base de datos tiene tablas optimizadas para memoria creadas con una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o en un nivel de compatibilidad por debajo de 130, las estadísticas se tendrán que actualizar manualmente una vez para, así, poder actualizarlas automáticamente desde ese instante.

Haga lo siguiente para habilitar la actualización automática de estadísticas de tablas optimizadas para memoria creadas en un nivel de compatibilidad anterior:

1. Actualice el nivel de compatibilidad de la base de datos: `ALTER DATABASE CURRENT SET COMPATIBILITY_LEVEL=130`

2. Actualice manualmente las estadísticas de las tablas optimizadas para memoria. Este es un script de ejemplo que consigue lo mismo.

3. Vuelva a compilar los procedimientos almacenados compilados de forma nativa para aprovechar las estadísticas actualizadas.

*Script único para estadísticas:* en el caso de las tablas optimizadas para memoria creadas en un nivel de compatibilidad inferior, se puede ejecutar el siguiente script Transact-SQL una vez para actualizar las estadísticas de todas esas tablas optimizadas para memoria y habilitar la actualización automática de estadísticas desde ese momento (suponiendo que AUTO_UPDATE_STATISTICS esté habilitado para la base de datos):

```
-- Assuming AUTO_UPDATE_STATISTICS is already ON for your database:
-- ALTER DATABASE CURRENT SET AUTO_UPDATE_STATISTICS ON;

ALTER DATABASE CURRENT SET COMPATIBILITY_LEVEL = 130;
GO
DECLARE @sql NVARCHAR(MAX) = N'';
SELECT
      @sql += N'UPDATE STATISTICS '
         + quotename(schema_name(t.schema_id))
         + N'.'
         + quotename(t.name)
         + ';' + CHAR(13) + CHAR(10)
   FROM sys.tables AS t
   WHERE t.is_memory_optimized = 1 AND 
        t.object_id IN (SELECT object_id FROM sys.stats WHERE no_recompute=1)
;
EXECUTE sp_executesql @sql;
GO
-- Each row appended to @sql looks roughly like:
-- UPDATE STATISTICS [dbo].[MyMemoryOptimizedTable];
```

*Comprobar que la actualización automática está habilitada:* con el siguiente script se comprueba si la actualización automática está habilitada para las estadísticas en las tablas optimizadas para memoria. Tras ejecutar el script anterior, devolverá `1` en la columna `auto-update enabled` de todos los objetos de estadística.

```
SELECT 
    quotename(schema_name(o.schema_id)) + N'.' + quotename(o.name) AS [table],
    s.name AS [statistics object],
    1-s.no_recompute AS [auto-update enabled]
FROM sys.stats s JOIN sys.tables o ON s.object_id=o.object_id
WHERE o.is_memory_optimized=1
```

## <a name="guidelines-for-deploying-tables-and-procedures"></a>Directrices para implementar tablas y procedimientos  
 Para asegurarse de que el optimizador de consultas dispone de estadísticas actualizadas al crear los planes de consulta, haga lo siguiente para implementar tablas optimizadas para memoria y procedimientos almacenados compilados de forma nativa con acceso a dichas tablas:  
  
1.  Procure que el nivel de compatibilidad de la base de datos sea al menos 130. Vea [Nivel de compatibilidad de ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

2.  Cree tablas e índices. Los índices han de especificarse insertados en instrucciones **CREATE TABLE** .  
  
3.  Cargue datos en las tablas.  
  
4.  Cree procedimientos almacenados que tengan acceso a las tablas.  
  
 El hecho de crear procedimientos almacenados compilados de forma nativa después de cargar los datos garantiza que el optimizador va a disponer de estadísticas para las tablas optimizadas para memoria. Esto garantizará planes de consulta eficaces cuando se compile el procedimiento.  

## <a name="see-also"></a>Consulte también  
 [Tablas optimizadas para la memoria](./sample-database-for-in-memory-oltp.md)  
  
