---
title: Almacenamiento de documentos JSON en SQL Server o SQL Database
description: En este artículo se describe por qué y cómo almacenar e indexar documentos JSON en SQL Server o SQL Database y cómo optimizar las consultas en los documentos JSON.
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 30d51162b36d787e8976b2c6aa48e533fd476f8e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015065"
---
# <a name="store-json-documents-in-sql-server-or-sql-database"></a>Almacenamiento de documentos JSON en SQL Server o SQL Database
SQL Server y Azure SQL Database tienen funciones JSON nativas que permiten analizar documentos JSON con el lenguaje SQL estándar. Ahora puede almacenar documentos JSON en SQL Server o SQL Database y consultar datos JSON como en una base de datos NoSQL. En este artículo se describen las opciones para almacenar documentos JSON en SQL Server o SQL Database.

## <a name="json-storage-format"></a>Formato de almacenamiento JSON

La primera decisión sobre el diseño del almacenamiento es cómo almacenar documentos JSON en las tablas. Hay dos opciones disponibles:
- **Almacenamiento de LOB**: los documentos JSON se pueden almacenar tal cual en las columnas `NVARCHAR`. Esta es la mejor manera para realizar la ingesta y carga rápida de datos, porque la velocidad de carga coincide con la carga de las columnas de cadena. Este enfoque podría significar una reducción adicional del rendimiento en tiempo de consulta/análisis si no se realiza la indexación en valores JSON, porque los documentos JSON sin formato se deben analizar mientras se ejecutan las consultas. 
- **Almacenamiento relacional**: los documentos JSON se pueden analizar mientras se insertan en la tabla con las funciones `OPENJSON`, `JSON_VALUE` o `JSON_QUERY`. Es posible almacenar fragmentos de los documentos JSON de entrada en las columnas de tipo de datos SQL o en columnas NVARCHAR que contienen subelementos JSON. Este enfoque aumenta el tiempo de carga, porque el análisis de JSON se realiza durante la carga. Sin embargo, las consultas coinciden con el rendimiento de las consultas clásicas en los datos relacionales.

## <a name="classic-tables"></a>Tablas clásicas

La manera más sencilla para almacenar documentos JSON en SQL Server o SQL Database es crear una tabla de dos columnas que contenga el id. del documento y el contenido de este. Por ejemplo:

```sql
create table WebSite.Logs (
    _id bigint primary key identity,
    log nvarchar(max)
);
```

Esta estructura es equivalente a las colecciones que se pueden encontrar en las bases de datos de documento clásicas. La clave principal `_id` es un valor de incremento automático que proporciona un identificador único para cada documento y habilita las búsquedas rápidas. Esta estructura es una buena elección para los escenarios NoSQL clásicos en los que quiera recuperar un documento mediante su id. o actualizar un documento almacenado mediante este.

El tipo de datos nvarchar (max) permite almacenar documentos JSON de hasta 2 GB de tamaño. Aunque esté seguro de que los documentos JSON no ocupan más de 8 KB, le recomendamos que use NVARCHAR (4000) en lugar de NVARCHAR (max) por motivos de rendimiento.

En la tabla de ejemplo creada en el ejemplo anterior se supone que los documentos JSON válidos se almacenan en la columna `log`. Si quiere estar seguro de que el JSON válido se guarda en la columna `log`, puede agregar una restricción CHECK en la columna. Por ejemplo:

```sql
ALTER TABLE WebSite.Logs
    ADD CONSTRAINT [Log record should be formatted as JSON]
                   CHECK (ISJSON(log)=1)
```

Cada vez que alguien inserta o actualiza un documento en la tabla, esta restricción comprueba que el documento JSON tiene un formato correcto. Sin la restricción, la tabla está optimizada para inserciones, porque cualquier documento JSON se agrega directamente a la columna sin procesarse.

Al almacenar los documentos JSON en la tabla, puede usar lenguaje Transact-SQL estándar para realizar consultas a los documentos. Por ejemplo:

```sql
SELECT TOP 100 JSON_VALUE(log, '$.severity'), AVG( CAST( JSON_VALUE(log,'$.duration') as float))
 FROM WebSite.Logs
 WHERE CAST( JSON_VALUE(log,'$.date') as datetime) > @datetime
 GROUP BY JSON_VALUE(log, '$.severity')
 HAVING AVG( CAST( JSON_VALUE(log,'$.duration') as float) ) > 100
 ORDER BY AVG( CAST( JSON_VALUE(log,'$.duration') as float) ) DESC
```

El poder usar *cualquier* función de T-SQL y realizar consultas a cláusulas para realizar consultas a documentos JSON es una gran ventaja. SQL Server y SQL Database no introducen ninguna restricción en las consultas que se pueden usar para analizar documentos JSON. Puede extraer los valores de un documento JSON con la función `JSON_VALUE` y usarlos en la consulta como cualquier otro valor.

Esta posibilidad de poder usar sintaxis de consulta T-SQL enriquecida es la diferencia principal entre SQL Server y SQL Database, y las bases de datos NoSQL clásicas: en Transact-SQL seguramente tenga alguna función que puede necesitar para procesar datos JSON.

## <a name="indexes"></a>Índices

Si descubre que las consultas buscan con frecuencia documentos por alguna propiedad (por ejemplo, una propiedad `severity` en un documento JSON), puede agregar un índice no agrupado clásico a la propiedad para acelerar las consultas.

Puede crear una columna calculada que exponga valores JSON de las columnas JSON en la ruta de acceso especificada (es decir, en la ruta de acceso `$.severity`) y crear un índice estándar en esta columna calculada. Por ejemplo:

```sql
create table WebSite.Logs (
    _id bigint primary key identity,
    log nvarchar(max),

    severity AS JSON_VALUE(log, '$.severity'),
    index ix_severity (severity)
);
```

La columna calculada usada en este ejemplo es una columna virtual o no persistente que no agrega espacio adicional a la tabla. La usa el índice `ix_severity` para mejorar el rendimiento de las consultas como en el siguiente ejemplo:

```sql
SELECT log
FROM Website.Logs
WHERE JSON_VALUE(log, '$.severity') = 'P4'
```

Una característica importante de este índice es que es compatible con la intercalación. Si la columna NVARCHAR original tiene una propiedad COLLATION (por ejemplo, idioma japonés o diferenciación entre mayúsculas y minúsculas), el índice se organiza según las reglas del idioma o las de diferenciación entre mayúsculas y minúsculas asociadas con la columna NVARCHAR. Este reconocimiento de intercalaciones puede ser una característica importante si está desarrollando aplicaciones para mercados globales que necesiten usar reglas de idioma personalizadas al procesar documentos JSON.

## <a name="large-tables--columnstore-format"></a>Formato de almacén de columnas y tablas grandes

Si espera tener una gran cantidad de documentos JSON en la colección, le recomendamos que agregue un índice de almacén de columnas agrupadas a la colección, tal y como se muestra en el siguiente ejemplo:

```sql
create sequence WebSite.LogID as bigint;
go
create table WebSite.Logs (
    _id bigint default(next value for WebSite.LogID),
    log nvarchar(max),

    INDEX cci CLUSTERED COLUMNSTORE
);
```

Un índice de almacén de columnas agrupadas proporciona alta compresión de datos (hasta 25 veces), lo que puede reducir significativamente los requisitos de espacio de almacenamiento, reducir el coste de este y aumentar el rendimiento de E/S de la carga de trabajo. Además, los índices de almacén de columnas agrupadas están optimizados para análisis y recorridos de tablas en los documentos JSON, por lo que este tipo de índice puede ser la mejor opción para registrar análisis.

En el ejemplo anterior se usa un objeto de secuencia para asignar valores a la columna `_id`. Tanto las secuencias como las identidades son opciones válidas para la columna ID.

## <a name="frequently-changing-documents--memory-optimized-tables"></a>Tablas optimizadas para memoria y documentos que cambian con frecuencia

Si espera que se realice un gran número de operaciones de actualización, inserción y eliminación en las colecciones, puede almacenar los documentos JSON en tablas optimizadas para memoria. Las colecciones JSON optimizadas para memoria siempre mantienen los datos en la memoria, por lo que no se producen sobrecargas en la E/S de almacenamiento. Además, las colecciones JSON optimizadas para memoria no tienen ningún tipo de bloqueo, por lo que las acciones de los documentos no bloquearán ninguna otra operación.

Lo único que puede hacer para convertir una colección clásica en una colección optimizada para memoria es especificar la opción **with (memory_optimized=on)** después de la definición de tabla, tal y como se muestra en el ejemplo siguiente. Así conseguirá una versión optimizada para memoria de la colección JSON.

```sql
create table WebSite.Logs (
  _id bigint identity primary key nonclustered,
  log nvarchar(4000)
) with (memory_optimized=on)
```

Una tabla optimizada para memoria es la mejor opción para documentos que sufren cambios frecuentes. En los casos de tablas optimizadas para memoria, también es importante tener en cuenta el rendimiento. Use NVARCHAR(4000) en vez de NVARCHAR(max) para los documentos JSON en las colecciones optimizadas para memoria, si es posible, ya que puede aumentar increíblemente el rendimiento.

Como sucede con las tablas clásicas, puede agregar índices en los campos que expone en tablas optimizadas para memoria con columnas calculadas. Por ejemplo:

```sql
create table WebSite.Logs (

  _id bigint identity primary key nonclustered,
  log nvarchar(4000),

  severity AS cast(JSON_VALUE(log, '$.severity') as tinyint) persisted,
  index ix_severity (severity)

) with (memory_optimized=on)
```

Para maximizar el rendimiento, convierta el valor JSON en el tipo más pequeño posible que pueda usarse para retener el valor de la propiedad. En el ejemplo anterior, se usa **tinyint**.

También puede colocar consultas de SQL que actualicen los documentos JSON en procedimientos almacenados para obtener las ventajas de la compilación nativa. Por ejemplo:

```sql
CREATE PROCEDURE WebSite.UpdateData(@Id int, @Property nvarchar(100), @Value nvarchar(100))
WITH SCHEMABINDING, NATIVE_COMPILATION

AS BEGIN
    ATOMIC WITH (transaction isolation level = snapshot,  language = N'English')

    UPDATE WebSite.Logs
    SET log = JSON_MODIFY(log, @Property, @Value)
    WHERE _id = @Id;

END
```

En este procedimiento compilado de forma nativa se toma la consulta y se crea código .DLL que ejecuta la consulta. Un procedimiento compilado de forma nativa es el enfoque más rápido para realizar consultas y actualizar datos.

## <a name="conclusion"></a>Conclusión

Las funciones JSON nativas en SQL Server y SQL Database le permiten procesar documentos JSON como si fuesen bases de datos NoSQL. Las bases de datos (relacionales o NoSQL) tienen sus puntos a favor y en contra para el procesamiento de datos JSON. La ventaja principal del almacenamiento de documentos JSON en SQL Server o SQL Database es la compatibilidad completa con lenguaje SQL. Puede usar el lenguaje enriquecido Transact-SQL para procesar datos y configurar varias opciones de almacenamiento (desde índices de almacén de columnas para grandes compresiones y análisis rápidos a tablas optimizadas para memoria para procesamiento sin bloqueos). Al mismo tiempo, obtendrá la ventaja de las características de internacionalización y seguridad madura que puede usar de forma sencilla en su escenario NoSQL. Las razones que se describen en este artículo son excelentes y se deben tener en cuenta a la hora de almacenar documentos JSON en SQL Server o SQL Database.

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>Más información sobre JSON en SQL Server y Azure SQL Database  
  
### <a name="microsoft-videos"></a>Vídeos de Microsoft

Para obtener una introducción visual a la compatibilidad integrada de JSON en SQL Server y Azure SQL Database, vea los siguientes vídeos:

-   [SQL Server 2016 and JSON Support](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support) (SQL Server 2016 y compatibilidad con JSON)

-   [Using JSON in SQL Server 2016 and Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database) (Uso de JSON en SQL Server 2016 y Azure SQL Database)

-   [JSON as a bridge between NoSQL and relational worlds](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) (JSON como puente entre los universos NoSQL y relacional)
