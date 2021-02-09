---
title: Uso de tipos de datos básicos de JDBC
description: Microsoft JDBC Driver para SQL Server usa tipos de datos básicos de JDBC para convertir los tipos de datos de SQL Server a un formato que pueda entenderse mediante Java.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: d7044936-5b8c-4def-858c-28a11ef70a97
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1c6f764fffca3408380da9c96227eb9d7bb4457c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195271"
---
# <a name="using-basic-data-types"></a>Empleo de tipos de datos básicos

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

[!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] usa los tipos de datos básicos de JDBC para convertir los tipos de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a un formato que el lenguaje de programación Java pueda entender y viceversa. El controlador JDBC proporciona compatibilidad con la API de JDBC 4.0, que incluye el tipo de datos **SQLXML** y tipos de datos nacionales (Unicode), como **NCHAR**, **NVARCHAR**, **LONGNVARCHAR** y **NCLOB**.  
  
## <a name="data-type-mappings"></a>Asignaciones de tipo de datos

En la siguiente tabla se muestran las asignaciones predeterminadas entre los tipos de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] básicos, JDBC y del lenguaje de programación Java:  
  
| Tipos de SQL Server   | Tipos de JDBC (Tipos de java.sql.)                        | Tipos del lenguaje Java          |
| ------------------ | -------------------------------------------------- | ---------------------------- |
| bigint             | bigint                                             | long                         |
| binary             | BINARY                                             | byte[]                       |
| bit                | BIT                                                | boolean                      |
| char               | CHAR                                               | String                       |
| date               | DATE                                               | java.sql.Date                |
| datetime<sup>3</sup>          | timestamp                               | java.sql.Timestamp           |
| datetime2          | timestamp                                          | java.sql.Timestamp           |
| datetimeoffset<sup>2</sup> | microsoft.sql.Types.DATETIMEOFFSET         | microsoft.sql.DateTimeOffset |
| Decimal            | DECIMAL                                            | java.math.BigDecimal         |
| FLOAT              | DOUBLE                                             | double                       |
| imagen              | LONGVARBINARY                                      | byte[]                       |
| int                | INTEGER                                            | int                          |
| money              | DECIMAL                                            | java.math.BigDecimal         |
| NCHAR              | CHAR<br /><br /> NCHAR (Java SE 6.0)               | String                       |
| ntext              | LONGVARCHAR<br /><br /> LONGNVARCHAR (Java SE 6.0) | String                       |
| NUMERIC            | NUMERIC                                            | java.math.BigDecimal         |
| NVARCHAR           | VARCHAR<br /><br /> NVARCHAR (Java SE 6.0)         | String                       |
| nvarchar(max)      | VARCHAR<br /><br /> NVARCHAR (Java SE 6.0)         | String                       |
| real               | real                                               | FLOAT                        |
| smalldatetime      | timestamp                                          | java.sql.Timestamp           |
| SMALLINT           | SMALLINT                                           | short                        |
| SMALLMONEY         | DECIMAL                                            | java.math.BigDecimal         |
| text               | LONGVARCHAR                                        | String                       |
| time               | TIME<sup>1</sup>                                   | java.sql.Time<sup>1</sup>            |
| timestamp          | BINARY                                             | byte[]                       |
| TINYINT            | TINYINT                                            | short                        |
| udt                | VARBINARY                                          | byte[]                       |
| UNIQUEIDENTIFIER   | CHAR                                               | String                       |
| varbinary          | VARBINARY                                          | byte[]                       |
| varbinary(max)     | VARBINARY                                          | byte[]                       |
| varchar            | VARCHAR                                            | String                       |
| ntext       | VARCHAR                                            | String                       |
| Xml                | LONGVARCHAR<br /><br /> LONGNVARCHAR (Java SE 6.0) | String<br /><br /> SQLXML    |
| sqlvariant         | microsoft.sql.Types.SQL_VARIANT                    | Object                       |
| geometry           | VARBINARY                                          | byte[]                       |
| geography          | VARBINARY                                          | byte[]                       |
  
<sup>1</sup> Para usar java.sql.Time con el tipo de hora [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], debe establecer la propiedad de conexión **sendTimeAsDatetime** en False.  
  
<sup>2</sup> Puede tener acceso mediante programación a los valores de **datetimeoffset** con la clase [DateTimeOffset](reference/datetimeoffset-class.md).  
  
<sup>3</sup> Tenga en cuenta que los valores java.sql.Timestamp ya no se pueden usar para comparar los valores de una columna datetime a partir de SQL Server 2016. Esta limitación se debe a un cambio en el servidor que convierte datetime2 en datetime2 de manera diferente, lo que da como resultado valores no equitativos. La solución a este problema es cambiar las columnas datetime a datetime2(3), usar una cadena en lugar de java. sql.Timestamp o cambiar el nivel de compatibilidad de la base de datos a 120 o inferior.
  
Las siguientes secciones proporcionan ejemplos de cómo puede usar el controlador JDBC y los tipos de datos básicos. Si quiere obtener un ejemplo detallado sobre cómo usar los tipos de datos básicos en una aplicación de Java, consulte [Ejemplo de tipos de datos básicos](basic-data-types-sample.md).  
  
## <a name="retrieving-data-as-a-string"></a>Recuperar datos como una cadena

Si tiene que recuperar datos de un origen de datos que se asigne a cualquiera de los tipos de datos básicos de JDBC para verlos como una cadena, o si no se necesitan datos fuertemente tipados, puede usar el método [getString](reference/getstring-method-sqlserverresultset.md) de la clase [SQLServerResultSet](reference/sqlserverresultset-class.md), como en el siguiente ejemplo:  
  
[!code[JDBC#UsingBasicDataTypes1](codesnippet/Java/using-basic-data-types_1.java)]  
  
## <a name="retrieving-data-by-data-type"></a>Recuperar datos por tipo de datos

Si tiene que recuperar datos de un origen de datos y sabe el tipo de datos que se van a recuperar, use uno de los métodos get\<Type> de la clase SQLServerResultSet, también conocidos como *métodos de captador*. Con los métodos get\<Type>, puede usar un nombre de columna o un índice de columna, como en el siguiente ejemplo:  
  
[!code[JDBC#UsingBasicDataTypes2](codesnippet/Java/using-basic-data-types_2.java)]  
  
> [!NOTE]  
> Los métodos con escala getUnicodeStream y getBigDecimal están obsoletos y el controlador JDBC no los admite.

## <a name="updating-data-by-data-type"></a>Actualizar datos por tipo de datos

Si tiene que actualizar el valor de un campo en un origen de datos, use uno de los métodos update\<Type> de la clase SQLServerResultSet. En el siguiente ejemplo, se usa el método [updateInt](reference/updateint-method-sqlserverresultset.md) en conjunción con el método [updateRow](reference/updaterow-method-sqlserverresultset.md) para actualizar los datos del origen de datos:  
  
[!code[JDBC#UsingBasicDataTypes3](codesnippet/Java/using-basic-data-types_3.java)]  
  
> [!NOTE]  
> El controlador JDBC no puede actualizar una columna SQL Server con un nombre de columna que tenga más de 127 caracteres de largo. Si se intenta una actualización a una columna cuyo nombre tenga más de 127 caracteres, se genera una excepción.  
  
## <a name="updating-data-by-parameterized-query"></a>Actualizar datos por consulta con parámetros

Si tiene que actualizar datos de un origen de datos mediante el uso de una consulta con parámetros, puede establecer el tipo de datos de los parámetros con los métodos set\<Type> de la clase [SQLServerPreparedStatement](reference/sqlserverpreparedstatement-class.md), también conocidos como *métodos de establecedor*. En el siguiente ejemplo, se usa el método [prepareStatement](reference/preparestatement-method-sqlserverconnection.md) para precompilar la consulta con parámetros y, luego, el método [setString](reference/setstring-method-sqlserverpreparedstatement.md) para establecer el valor de cadena del parámetro antes de llamar al método [executeUpdate](reference/executeupdate-method.md).  
  
[!code[JDBC#UsingBasicDataTypes4](codesnippet/Java/using-basic-data-types_4.java)]  
  
Para obtener más información sobre las consultas con parámetros, consulte [Usar una instrucción SQL con parámetros](using-an-sql-statement-with-parameters.md).  

## <a name="passing-parameters-to-a-stored-procedure"></a>Pasar parámetros a un procedimiento almacenado

Si tiene que pasar parámetros tipados a un procedimiento almacenado, puede establecerlos por índice o por nombre mediante el uso de uno de los métodos set\<Type> de la clase [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md). En el siguiente ejemplo, se usa el método [prepareCall](../../connect/jdbc/reference/preparecall-method-sqlserverconnection.md) para configurar la llamada al procedimiento almacenado y, luego, el método [setString](../../connect/jdbc/reference/setstring-method-sqlservercallablestatement.md) para establecer el parámetro para la llamada antes de llamar al método [executeQuery](../../connect/jdbc/reference/executequery-method-sqlserverstatement.md).  
  
[!code[JDBC#UsingBasicDataTypes5](codesnippet/Java/using-basic-data-types_5.java)]  
  
> [!NOTE]  
> En este ejemplo, se devuelve un conjunto de resultados con los resultados de la ejecución del procedimiento almacenado.

Para obtener más información sobre el uso del controlador JDBC con procedimientos almacenados y parámetros de entrada, consulte [Empleo de un procedimiento almacenado con parámetros de entrada](using-a-stored-procedure-with-input-parameters.md).  

## <a name="retrieving-parameters-from-a-stored-procedure"></a>Recuperar parámetros desde un procedimiento almacenado

Si tiene que recuperar parámetros de un procedimiento almacenado, primero debe registrar un parámetro de salida por el nombre o el índice con el método [registerOutParameter](../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md) de la clase SQLServerCallableStatement y, luego, asignar el parámetro de salida devuelto a una variable adecuada tras haber ejecutado la llamada al procedimiento almacenado. En el siguiente ejemplo, se usa el método prepareCall para configurar la llamada al procedimiento almacenado, el método registerOutParameter para configurar el parámetro de salida y el método [setString](../../connect/jdbc/reference/setstring-method-sqlservercallablestatement.md) para establecer el parámetro para la llamada antes de llamar al método executeQuery. El valor que devuelve el parámetro de salida del procedimiento almacenado se recupera con el método [getShort](../../connect/jdbc/reference/getshort-method-sqlservercallablestatement.md).  
  
[!code[JDBC#UsingBasicDataTypes6](../../connect/jdbc/codesnippet/Java/using-basic-data-types_6.java)]  
  
> [!NOTE]  
> Además del parámetro de salida devuelto, puede que también se devuelva un conjunto de resultados con los resultados de ejecución del procedimiento almacenado.  
  
Para obtener más información sobre el uso del controlador JDBC con procedimientos almacenados y parámetros de salida, consulte [Empleo de un procedimiento almacenado con parámetros de salida](../../connect/jdbc/using-a-stored-procedure-with-output-parameters.md).  

## <a name="see-also"></a>Consulte también

[Descripción de los tipos de datos del controlador JDBC](../../connect/jdbc/understanding-the-jdbc-driver-data-types.md)  
