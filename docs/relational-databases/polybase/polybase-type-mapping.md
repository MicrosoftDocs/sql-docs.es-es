---
title: Asignación de tipos con PolyBase | Microsoft Docs
description: Consulte estas tablas para obtener la asignación entre los orígenes de datos externos de PolyBase y SQL Server. Defina las tablas externas con CREATE EXTERNAL TABLE de Transact-SQL.
ms.date: 09/24/2018
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
ms.openlocfilehash: ddb0c0ac0c1afd4e4a3f94504256fa787dceaf9d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080018"
---
# <a name="type-mapping-with-polybase"></a>Asignación de tipos con PolyBase

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

En este artículo se describe la asignación entre los orígenes de datos externos de PolyBase y SQL Server. Puede usar esta información para definir correctamente las tablas externas con el comando de Transact-SQL [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md).

## <a name="overview"></a>Información general

A la hora de crear una tabla externa con PolyBase, las definiciones de columna, incluidos los tipos de datos y el número de columnas, deben coincidir con los datos de los archivos externos. Si hay alguna no coincidencia, se rechazarán las filas de archivo al consultar los datos reales.  
  
En tablas externas que hacen referencia a archivos de orígenes de datos externos, las definiciones de columna y tipo deben asignarse al esquema exacto del archivo externo. Al definir tipos de datos que hacen referencia a datos almacenados en Hadoop/Hive, use las siguientes asignaciones entre tipos de datos SQL y Hive y convierta el tipo a un tipo de datos SQL cuando seleccione desde él. Los tipos incluyen todas las versiones de Hive, a menos que se indique lo contrario.

> [!NOTE]  
> SQL Server no admite el valor de datos *infinity* de Hive en ninguna conversión. En PolyBase se produce un error de conversión de tipo de datos.

## <a name="hadoop-type-mapping-reference"></a>Referencia de asignación de tipos de Hadoop

| Tipo de datos de SQL | Tipo de datos de .NET            | Tipo de datos de Hive | Hadoop/Java Data Type | Comentarios                       |
| ------------- | ------------------------- | -------------- | --------------------- | ------------------------------ |
| TINYINT       | Byte                      | TINYINT        | ByteWritable          | Solo para números sin signo.     |
| SMALLINT      | Int16                     | SMALLINT       | ShortWritable         |
| int           | Int32                     | int            | IntWritable           |
| bigint        | Int64                     | bigint         | LongWritable          |
| bit           | Boolean                   | boolean        | BooleanWritable       |
| FLOAT         | Double                    | double         | DoubleWritable        |
| real          | Single                    | FLOAT          | FloatWritable         |
| money         | Decimal                   | double         | DoubleWritable        |
| SMALLMONEY    | Decimal                   | double         | DoubleWritable        |
| NCHAR         | String<br /><br /> Char[] | string         | Varchar               |
| NVARCHAR      | String<br /><br /> Char[] | string         | Varchar               |
| char          | String<br /><br /> Char[] | string         | Varchar               |
| varchar       | String<br /><br /> Char[] | string         | Varchar               |
| binary        | Byte[]                    | binary         | BytesWritable         | Se aplica a Hive 0.8 y posterior. |
| varbinary     | Byte[]                    | binary         | BytesWritable         | Se aplica a Hive 0.8 y posterior. |
| date          | DateTime                  | timestamp      | TimestampWritable     |
| smalldatetime | DateTime                  | timestamp      | TimestampWritable     |
| datetime2     | DateTime                  | timestamp      | TimestampWritable     |
| datetime      | DateTime                  | timestamp      | TimestampWritable     |
| time          | TimeSpan                  | timestamp      | TimestampWritable     |
| Decimal       | Decimal                   | Decimal        | BigDecimalWritable    | Se aplica a Hive 0.11 y posterior. |

<!--SQL Server 2019-->
::: moniker range=">= sql-server-ver15 "

## <a name="oracle-type-mapping-reference"></a>Referencia de asignación de tipos de Oracle

| Tipo de datos de Oracle | Tipo de SQL Server | 
| -------------    | --------------- |
|Float             |Float            |
|NUMBER            |Float            |
|NUMBER (p,s)      |Decimal (p, s)   |
|LONG              |nvarchar         |
|BINARY_FLOAT      |Real             | 
|BINARY_DOUBLE     |Float            | 
|CHAR              |Char             |
|VARCHAR2          |Varchar          | 
|NVARCHAR2         |nvarchar         | 
|RAW               |Varbinary        |
|LONG RAW          |Varbinary        | 
|BLOB              |Varbinary        |
|CLOB              |Varchar          |
|NCLOB             | nvarchar        | 
|ROWID             |Varchar          |
|UROWID            |Varchar          | 
|DATE              |Datetime2        |
|timestamp         |Datetime2        | 

**Error de coincidencia de tipos** 

**Float:** Oracle admite una precisión de punto flotante de 126, que es menor que la que admite SQL Server (53). Por lo tanto, **Float (1-53)** se puede asignar directamente, pero si se asigna un valor mayor, se experimentará una pérdida de datos debido al truncamiento.

**Marca de tiempo:** marca de tiempo y marca de tiempo con zona horaria local en Oracle admiten una precisión de 9 fracciones de segundo, mientras que SQL Server DateTime2 solo admite una precisión de 7 fracciones de segundo. 




## <a name="mongodb-type-mapping"></a>Asignación de tipo MongoDB

| Tipo de datos BSON     | Tipo de SQL Server |
| ------------------ | --------------- |
| Double             | Float           |
| String             | nvarchar        |
| Datos binarios        | nvarchar        |
| Id. de objeto          | nvarchar        |
| Boolean            | bit             |
| Date               | Datetime2       |
| Entero de 32 bits     | Int             |
| Timestamp          | nvarchar        |
| Entero de 64 bits     | BigInt          |
|Decimal 128         | Decimal         | 
| DBPointer          | nvarchar        |
| JavaScript         | nvarchar        |
| Clave Max            | nvarchar        |
| Clave Min            | nvarchar        |
| Símbolo             | nvarchar        |
| Expresión regular | nvarchar        |
| Sin definir/NULL     | nvarchar        |


MongoDB usa documentos BSON para almacenar los registros de datos. A diferencia de los escenarios anteriores, BSON carece de esquema y admite la incrustación de documentos y matrices en otros documentos. Esto proporciona flexibilidad al usuario. 


## <a name="teradata-type-mapping-reference"></a>Referencia de asignación de tipos de Teradata

| Tipo de datos de Teradata | Tipo de SQL Server | 
| -------------      | -------------   |
|INTEGER             |Int              |
|SMALLINT            |SmallInt         |
|bigint              |BigInt           |
|BYTEINT             |SmallInt         |
|DECIMAL             |Decimal          |
|FLOAT               |Decimal          |
|BYTE                |Binary           |
|VARBYTE             |Varbinary        |
|BLOB                |varbinary        |
|CHAR                |Nchar            |
|CLOB                |nvarchar         |
|VARCHAR             |nvarchar         |
|Graphic             |Nchar            |
|JSON                |nvarchar         |
|VARGRAPHIC          |nvarchar         |
|DATE                |Date             |
|timestamp           |Datetime2        |
|TIME                |Time             |
|TIME WITH TIME ZONE |Time             |
|TIMESTAMP WITH TIME ZONE|Hora         |

::: moniker-end

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo se usa, consulte el artículo de referencia de Transact-SQL relativo a [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md).
