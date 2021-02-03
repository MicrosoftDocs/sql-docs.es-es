---
description: COLLATE (Transact-SQL)
title: COLLATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- COLLATE
- COLLATE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- collations [SQL Server], COLLATE clause
- COLLATE clause
ms.assetid: 76763ac8-3e0d-4bbb-aa53-f5e7da021daa
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d84dffab852ed071b75db0bef34d5db488a891f3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184473"
---
# <a name="collate-transact-sql"></a>COLLATE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Define una intercalación de una base de datos o una columna de tabla, o una operación de conversión de intercalación cuando se aplica a una expresión de cadena de caracteres. El nombre de intercalación puede ser un nombre de intercalación de Windows o un nombre de intercalación de SQL. Si no se especifica durante la creación de la base de datos, a esta se le asigna la intercalación predeterminada de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si no se especifica durante la creación de la columna de tabla, a la columna se le asigna la intercalación predeterminada de la base de datos.

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintaxis

```syntaxsql
COLLATE { <collation_name> | database_default }
<collation_name> :: =
    { Windows_collation_name } | { SQL_collation_name }
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

*collation_name* Es el nombre de la intercalación que se va a aplicar a la expresión, definición de columna o definición de base de datos. *collation_name* solamente puede ser un *Windows_collation_name* o un *SQL_collation_name* especificado. *collation_name* debe ser un valor literal. *collation_name* no se puede representar con una variable ni una expresión.

*Windows_collation_name* es el nombre de intercalación de un [nombre de intercalación de Windows](../../t-sql/statements/windows-collation-name-transact-sql.md).

*SQL_collation_name* es el nombre de intercalación de un [nombre de intercalación de SQL Server](../../t-sql/statements/sql-server-collation-name-transact-sql.md).

**database_default** Hace que la cláusula COLLATE herede la intercalación de la base de datos actual.

## <a name="remarks"></a>Observaciones

La cláusula COLLATE se puede especificar en varios niveles. Entre ellas, se incluyen las siguientes:

1. Crear o modificar una base de datos.

    Puede usar la cláusula COLLATE de la instrucción `CREATE DATABASE` o `ALTER DATABASE` para especificar la intercalación predeterminada de la base de datos. También puede especificar una intercalación al crear una base de datos utilizando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Si no especifica ninguna intercalación, se asigna a la base de datos la intercalación predeterminada de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

    > [!NOTE]
    > Las intercalaciones solo Unicode de Windows se pueden usar únicamente con la cláusula COLLATE para aplicar intercalaciones a los tipos de datos **nchar**, **nvarchar** y **ntext** de datos de nivel de columna y nivel de expresión; no se pueden usar con la cláusula COLLATE para definir ni cambiar la intercalación de una instancia de base de datos o servidor.

2. Crear o modificar una columna de una tabla.

    Puede especificar intercalaciones para cada columna de cadena de caracteres mediante la cláusula COLLATE de la instrucción `CREATE TABLE` o `ALTER TABLE`. También puede especificar una intercalación al crear una tabla utilizando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Si no especifica ninguna intercalación, se asigna a la columna la intercalación predeterminada de la base de datos.

    También puede usar la opción `database_default` de la cláusula COLLATE para especificar que una columna de una tabla temporal use la intercalación predeterminada de la base de datos de usuario actual para la conexión en lugar de usar **tempdb**.

3. Convertir la intercalación de una expresión.

    Puede utilizar la cláusula COLLATE para aplicar una expresión de caracteres a una intercalación concreta. La intercalación predeterminada de la base de datos actual se asigna a los literales y las variables de carácter. La intercalación de definición de la columna se asigna a las referencias de columna.

La intercalación de un identificador depende del nivel en que está definido. Se asigna a los identificadores de objetos de instancia, como los inicios de sesión y los nombres de base de datos, la intercalación predeterminada de la instancia. Se asigna a los identificadores de objetos de una base de datos, como nombres de tablas, vistas y columnas, la intercalación predeterminada de la base de datos. Por ejemplo, es posible crear dos tablas con nombres que solo se diferencian en las mayúsculas en una base de datos con intercalación que distinga entre mayúsculas y minúsculas, pero no se pueden crear en una base de datos con una intercalación que no distinga entre mayúsculas y minúsculas. Para obtener más información, vea [Database Identifiers](../../relational-databases/databases/database-identifiers.md).

Las variables, etiquetas GOTO, procedimientos almacenados temporales y tablas temporales pueden crearse cuando se asocia el contexto de conexión a una base de datos y, a continuación, se les hace referencia cuando se ha cambiado el contexto a otra base de datos. Los identificadores para variables, etiquetas GOTO, procedimientos almacenados temporales y tablas temporales se encuentran en la intercalación predeterminada de la instancia de servidor.

La cláusula COLLATE se puede aplicar únicamente a los tipos de datos **char**, **varchar**, **text**, **nchar**, **nvarchar** y **ntext**.

COLLATE usa *collate_name* para hacer referencia al nombre de la intercalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o de la intercalación de Windows que se va a aplicar a la expresión, definición de columna o definición de base de datos. *collation_name* solamente puede ser un *Windows_collation_name* o un *SQL_collation_name* especificado y el parámetro debe contener un valor literal. *collation_name* no se puede representar con una variable ni una expresión.

Las intercalaciones se suelen identificar mediante un nombre de intercalación, excepto en el programa de instalación. En el programa de instalación, se especifica en su lugar el designador de intercalación raíz (configuración regional de la intercalación) para las intercalaciones de Windows y, después, se especifican las opciones de ordenación que distinguen o no las mayúsculas de minúsculas, o acentos.

Puede ejecutar la función del sistema [fn_helpcollations](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md) para recuperar una lista de todos los nombres de intercalación válidos para intercalaciones de Windows y de SQL Server:

```sql
SELECT name, description
FROM fn_helpcollations();
```

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] solo admite páginas de códigos compatibles con el sistema operativo subyacente. Cuando ejecuta una acción que depende de intercalaciones, la intercalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizada por el objeto de referencia debe usar una página de códigos compatible con el sistema operativo del equipo. Entre estas acciones pueden incluirse las siguientes:

- Especificar una intercalación predeterminada para una base de datos durante su creación o modificación.
- Especificar una intercalación para una columna durante la creación o la modificación de una tabla.
- Cuando se restaura o anexa una base de datos, la intercalación predeterminada de la base de datos y la intercalación de las columnas **char**, **varchar** y **text** o los parámetros de la base de datos deben ser compatibles con el sistema operativo.

> [!NOTE]
> Las traducciones de páginas de códigos se admiten para los tipos de datos **char** y **varchar**, pero no para el tipo de datos **text**. La pérdida de datos durante la traducción de páginas de códigos no se notifica.
>
> Si la intercalación especificada o la intercalación usada por el objeto al que se hace referencia usa una página de códigos no admitida por Windows, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] muestra un error.

## <a name="examples"></a>Ejemplos

### <a name="a-specifying-collation-during-a-select"></a>A. Especificar la intercalación durante una operación SELECT

En el ejemplo siguiente se crea la tabla simple y se insertan 4 filas. A continuación, el ejemplo aplica dos intercalaciones al seleccionar datos de la tabla, mostrando cómo `Chiapas` se ordena de manera diferente.

```sql
CREATE TABLE Locations
(Place varchar(15) NOT NULL);
GO
INSERT Locations(Place) VALUES ('Chiapas'),('Colima')
                             , ('Cinco Rios'), ('California');
GO
--Apply an typical collation
SELECT Place FROM Locations
ORDER BY Place
COLLATE Latin1_General_CS_AS_KS_WS ASC;
GO
-- Apply a Spanish collation
SELECT Place FROM Locations
ORDER BY Place
COLLATE Traditional_Spanish_ci_ai ASC;
GO
```

Estos son los resultados de la primera consulta.

```output
Place
-------------
California
Chiapas
Cinco Rios
Colima
```

Estos son los resultados de la segunda consulta.

```output
Place
-------------
California
Cinco Rios
Colima
Chiapas
```

### <a name="b-additional-examples"></a>B. Ejemplos adicionales

Para ver más ejemplos en los que se usa **COLLATE**, consulte el ejemplo de [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md#examples)**G. Crear una base de datos y especificar un nombre de intercalación y sus opciones**, y el ejemplo de [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md#alter_column)**V. Cambiar la intercalación de columnas**.

## <a name="see-also"></a>Consulte también

- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md)
- [Prioridad de intercalación](../../t-sql/statements/collation-precedence-transact-sql.md)
- [Constantes](../../t-sql/data-types/constants-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)
- [DECLARE @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md)
- [Tipo de datos de tabla](../../t-sql/data-types/table-transact-sql.md)
