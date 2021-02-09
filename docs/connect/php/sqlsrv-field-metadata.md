---
description: sqlsrv_field_metadata
title: sqlsrv_field_metadata | Microsoft Docs
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_field_metadata
apitype: NA
helpviewer_keywords:
- API Reference, sqlsrv_field_metadata
- sqlsrv_field_metadata
ms.assetid: c02f6942-0484-4567-a78e-fe8aa2053536
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5cbcb5cf689d544730661fd9dd700537309d8a23
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99154242"
---
# <a name="sqlsrv_field_metadata"></a>sqlsrv_field_metadata
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Recupera los metadatos de los campos de una instrucción preparada. Para obtener información acerca de cómo preparar una instrucción, consulte [sqlsrv_query](../../connect/php/sqlsrv-query.md) o [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md). Tenga en cuenta que se puede llamar a **sqlsrv_field_metadata** en cualquier instrucción preparada, ya sea antes o después de la ejecución.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sqlsrv_field_metadata( resource $stmt)  
```  
  
#### <a name="parameters"></a>Parámetros  
*$stmt*: un recurso de instrucción para el que se buscan metadatos de campo.  
  
## <a name="return-value"></a>Valor devuelto  
Una **matriz** de matrices o el valor **False**. La matriz consta de una matriz por cada campo del conjunto de resultados. Cada submatriz contiene las claves que se describen en la siguiente tabla. Si se produce un error en el proceso de recuperación de los metadatos de los campos, se devuelve el valor **False** .  
  
|Clave|Descripción|  
|-------|---------------|  
|NOMBRE|Nombre de la columna a la que corresponde el campo.|  
|Tipo|Valor numérico que corresponde a un tipo SQL.|  
|Tamaño|Número de caracteres de los campos de tipo de carácter: char(n), varchar(n), nchar(n), nvarchar(n) y XML. Número de bytes de los campos de tipo binario: binary(n), varbinary(n) y UDT. En otros tipos de datos de SQL Server, se devuelve **NULL** .|  
|Precisión|La precisión de los tipos de precisión de variables: real, numeric, decimal, datetime2, datetimeoffset y time. En otros tipos de datos de SQL Server, se devuelve **NULL** .|  
|Escala|La escala de los tipos de escala de variables: numeric, decimal, datetime2, datetimeoffset y time. En otros tipos de datos de SQL Server, se devuelve **NULL** .|  
|Nullable|Un valor enumerado que indica si la columna acepta valores Null (**SQLSRV_NULLABLE_YES**), no los acepta (**SQLSRV_NULLABLE_NO**) o no se sabe si los acepta (**SQLSRV_NULLABLE_UNKNOWN**).|  
  
En la tabla siguiente se proporciona más información sobre las claves de cada submatriz (consulte la documentación de SQL Server para obtener más información sobre estos tipos):  
  
|Tipo de datos de SQL Server 2008|Tipo|Precisión mín./máx.|Escala mín./máx.|Tamaño|  
|-----------------------------|--------|----------------------|------------------|--------|  
|bigint|SQL_BIGINT (-5)|||8|  
|binary|SQL_BINARY (-2)|||0 < *n* < 8000 <sup>1</sup>|  
|bit|SQL_BIT (-7)||||  
|char|SQL_CHAR (1)|||0 < *n* < 8000 <sup>1</sup>|  
|Fecha|SQL_TYPE_DATE (91)|10/10|0/0||  
|datetime|SQL_TYPE_TIMESTAMP (93)|23/23|3/3||  
|datetime2|SQL_TYPE_TIMESTAMP (93)|19/27|0/7||  
|datetimeoffset|SQL_SS_TIMESTAMPOFFSET (-155)|26/34|0/7||  
|Decimal|SQL_DECIMAL (3)|1/38|0/valor de precisión||  
|FLOAT|SQL_FLOAT (6)|4/8|||  
|imagen|SQL_LONGVARBINARY (-4)|||2 GB|  
|int|SQL_INTEGER (4)||||  
|money|SQL_DECIMAL (3)|19/19|4/4||  
|NCHAR|SQL_WCHAR (-8)|||0 < *n* < 4000 <sup>1</sup>|  
|ntext|SQL_WLONGVARCHAR (-10)|||1 GB|  
|NUMERIC|SQL_NUMERIC (2)|1/38|0/valor de precisión||  
|NVARCHAR|SQL_WVARCHAR (-9)|||0 < *n* < 4000 <sup>1</sup>|  
|real|SQL_REAL (7)|4/4|||  
|smalldatetime|SQL_TYPE_TIMESTAMP (93)|16/16|0/0||  
|SMALLINT|SQL_SMALLINT (5)|||2 bytes|  
|Smallmoney|SQL_DECIMAL (3)|10/10|4/4||  
|sql_variant|SQL_SS_VARIANT (-150)|||variable|  
|texto|SQL_LONGVARCHAR (-1)|||2 GB|  
|time|SQL_SS_TIME2 (-154)|8/16|0/7||  
|timestamp|SQL_BINARY (-2)|||8 bytes|  
|TINYINT|SQL_TINYINT (-6)|||1 byte|  
|udt|SQL_SS_UDT (-151)|||variable|  
|UNIQUEIDENTIFIER|SQL_GUID (-11)|||16|  
|varbinary|SQL_VARBINARY (-3)|||0 < *n* < 8000 <sup>1</sup>|  
|varchar|SQL_VARCHAR (12)|||0 < *n* < 8000 <sup>1</sup>|  
|Xml|SQL_SS_XML (-152)|||0|  
  
(1) Un valor de cero (0) indica que se permite el tamaño máximo.  
  
La clave que acepta valores Null puede ser yes o no.  
  
## <a name="example"></a>Ejemplo  
En el ejemplo siguiente se crea un recurso de instrucción y, luego, se recuperan y muestran los metadatos de los campos. En el ejemplo se da por hecho que SQL Server y la base de datos [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) están instalados en el equipo local. Los resultados se agregan a la consola cuando se ejecuta el ejemplo en la línea de comandos.  
  
```
<?php
/* Connect to the local server using Windows Authentication and
specify the AdventureWorks database as the database in use. */
$serverName = "(local)";
$connectionInfo = array("Database"=>"AdventureWorks");
$conn = sqlsrv_connect($serverName, $connectionInfo);
if ($conn === false) {
    echo "Could not connect.\n";
    die( print_r( sqlsrv_errors(), true));
}

/* Prepare the statement. */
$tsql = "SELECT ReviewerName, Comments FROM Production.ProductReview";
$stmt = sqlsrv_prepare( $conn, $tsql);
  
/* Get and display field metadata. */
foreach( sqlsrv_field_metadata( $stmt) as $fieldMetadata) {
    foreach( $fieldMetadata as $name => $value) {
        echo "$name: $value\n";
    }  
    echo "\n";
}  
  
/* Note: sqlsrv_field_metadata can be called on any statement
resource, pre- or post-execution. */
  
/* Free statement and connection resources. */
sqlsrv_free_stmt($stmt);
sqlsrv_close($conn);
?>
```

## <a name="sensitivity-data-classification-metadata"></a>Metadatos de clasificación de datos confidenciales

En la versión 5.8.0, se incluye una nueva opción `DataClassification` para que los usuarios tengan acceso a los [metadatos de clasificación de datos confidenciales](../../relational-databases/security/sql-data-discovery-and-classification.md?tabs=t-sql#subheading-4) en Microsoft SQL Server 2019 con `sqlsrv_field_metadata`, que requiere Microsoft ODBC Driver 17.4.2 o versiones posteriores.

De forma predeterminada, la opción `DataClassification` es `false`, pero cuando se establece en `true`, la matriz devuelta por `sqlsrv_field_metadata` se rellenará con los metadatos de la clasificación de datos confidenciales, si existe. 

Tome como ejemplo una tabla Patients (Pacientes):

```
CREATE TABLE Patients 
      [PatientId] int identity,
      [SSN] char(11),
      [FirstName] nvarchar(50),
      [LastName] nvarchar(50),
      [BirthDate] date)
```

Podemos clasificar las columnas SSN y BirthDate como se muestra a continuación:

```
ADD SENSITIVITY CLASSIFICATION TO [Patients].SSN WITH (LABEL = 'Highly Confidential - secure privacy', INFORMATION_TYPE = 'Credentials')
ADD SENSITIVITY CLASSIFICATION TO [Patients].BirthDate WITH (LABEL = 'Confidential Personal Data', INFORMATION_TYPE = 'Birthdays')
```

Para acceder a los metadatos, invoque `sqlsrv_field_metadata` como se muestra en el fragmento de código siguiente:

```
$tableName = 'Patients';
$tsql = "SELECT * FROM $tableName";
$stmt = sqlsrv_prepare($conn, $tsql, array(), array('DataClassification' => true));
if (sqlsrv_execute($stmt)) {
    $fieldmeta = sqlsrv_field_metadata($stmt);

    foreach ($fieldmeta as $f) {
        if (count($f['Data Classification']) > 0) {
            echo $f['Name'] . ": \n";
            print_r($f['Data Classification']); 
        }
    }
}
```

El resultado será:

```
SSN: 
Array
(
    [0] => Array
        (
            [Label] => Array
                (
                    [name] => Highly Confidential - secure privacy
                    [id] => 
                )

            [Information Type] => Array
                (
                    [name] => Credentials
                    [id] => 
                )

        )

)
BirthDate: 
Array
(
    [0] => Array
        (
            [Label] => Array
                (
                    [name] => Confidential Personal Data
                    [id] => 
                )

            [Information Type] => Array
                (
                    [name] => Birthdays
                    [id] => 
                )

        )

)
```

Si usa `sqlsrv_query` en lugar de `sqlsrv_prepare`, el fragmento de código anterior se puede modificar, como se indica a continuación:

```
$tableName = 'Patients';
$tsql = "SELECT * FROM $tableName";
$stmt = sqlsrv_query($conn, $tsql, array(), array('DataClassification' => true));
$fieldmeta = sqlsrv_field_metadata($stmt);

foreach ($fieldmeta as $f) {
    $jstr = json_encode($f);
    echo $jstr . PHP_EOL;
}
```

Como puede ver en la representación JSON siguiente, se muestran los metadatos de clasificación de datos si están asociados a las columnas:

```
{"Name":"PatientId","Type":4,"Size":null,"Precision":10,"Scale":null,"Nullable":0,"Data Classification":[]}
{"Name":"SSN","Type":1,"Size":11,"Precision":null,"Scale":null,"Nullable":1,"Data Classification":[{"Label":{"name":"Highly Confidential - secure privacy","id":""},"Information Type":{"name":"Credentials","id":""}}]}
{"Name":"FirstName","Type":-9,"Size":50,"Precision":null,"Scale":null,"Nullable":1,"Data Classification":[]}
{"Name":"LastName","Type":-9,"Size":50,"Precision":null,"Scale":null,"Nullable":1,"Data Classification":[]}
{"Name":"BirthDate","Type":91,"Size":null,"Precision":10,"Scale":0,"Nullable":1,"Data Classification":[{"Label":{"name":"Confidential Personal Data","id":""},"Information Type":{"name":"Birthdays","id":""}}]}
```

## <a name="sensitivity-rank-using-a-predefined-set-of-values"></a>Rango de confidencialidad con un conjunto predefinido de valores

A partir de la versión 5.9.0, los controladores de PHP incluyen la recuperación de rangos de clasificación al usar el controlador ODBC 17.4.2 o versiones posteriores. El usuario puede definir un rango al usar la instrucción [ADD SENSITIVITY CLASSIFICATION](/sql/t-sql/statements/add-sensitivity-classification-transact-sql) para clasificar cualquier columna de datos. 

Por ejemplo, si el usuario asigna `NONE` y `LOW` a BirthDate y SSN respectivamente, la representación JSON se muestra de la siguiente manera:

```
{"0":{"Label":{"name":"Confidential Personal Data","id":""},"Information Type":{"name":"Birthdays","id":""},"rank":0},"rank":0}
{"0":{"Label":{"name":"Highly Confidential - secure privacy","id":""},"Information Type":{"name":"Credentials","id":""},"rank":10},"rank":10}
```

Tal y como se muestra en [la clasificación de confidencialidad](/sql/relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql), los valores numéricos de los rangos son:

```
0 for NONE
10 for LOW
20 for MEDIUM
30 for HIGH
40 for CRITICAL
```

Por lo tanto, si en lugar de `RANK=NONE`, el usuario define `RANK=CRITICAL` al clasificar la columna BirthDate, los metadatos de clasificación serán los siguientes:

```
  array(7) {
    ["Name"]=>
    string(9) "BirthDate"
    ["Type"]=>
    int(91)
    ["Size"]=>
    NULL
    ["Precision"]=>
    int(10)
    ["Scale"]=>
    int(0)
    ["Nullable"]=>
    int(1)
    ["Data Classification"]=>
    array(2) {
      [0]=>
      array(3) {
        ["Label"]=>
        array(2) {
          ["name"]=>
          string(26) "Confidential Personal Data"
          ["id"]=>
          string(0) ""
        }
        ["Information Type"]=>
        array(2) {
          ["name"]=>
          string(9) "Birthdays"
          ["id"]=>
          string(0) ""
        }
        ["rank"]=>
        int(40)
      }
      ["rank"]=>
      int(40)
    }
  }
```

A continuación se muestra la representación JSON actualizada:

```
{"0":{"Label":{"name":"Confidential Personal Data","id":""},"Information Type":{"name":"Birthdays","id":""},"rank":40},"rank":40}
```

## <a name="see-also"></a>Consulte también  
[Referencia de API del controlador SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  

[Constantes &#40;controladores de Microsoft para PHP para SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md)  

[Sobre los ejemplos de código de la documentación](../../connect/php/about-code-examples-in-the-documentation.md)  
