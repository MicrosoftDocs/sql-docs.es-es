---
title: PDOStatement::getColumnMeta
description: Referencia de API de la función PDOStatement::getColumnMeta en el controlador PDO_SQLSRV de Microsoft para PHP en SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: c92a21cc-8e53-43d0-a4bf-542c77c100c9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c5793f486b43fe4c2d12ec9be004dbb2b3346020
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179925"
---
# <a name="pdostatementgetcolumnmeta"></a>PDOStatement::getColumnMeta
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Recupera los metadatos de una columna.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
array PDOStatement::getColumnMeta ( $column );  
```  
  
#### <a name="parameters"></a>Parámetros  
*$conn*: (entero) número basado en cero de la columna cuyos metadatos se van a recuperar.  
  
## <a name="return-value"></a>Valor devuelto  
Una matriz asociativa (clave y valor) que contiene los metadatos de la columna. Consulte la sección Comentarios para obtener una descripción de los campos de la matriz.  
  
## <a name="remarks"></a>Observaciones  
En la tabla siguiente se describen los campos de la matriz que devuelve getColumnMeta.  
  
|NOMBRE|VALUES|  
|--------|----------|  
|native_type|Especifica el tipo PHP de la columna. Siempre es una cadena.|  
|driver:decl_type|Especifica el tipo SQL que se utiliza para representar el valor de columna en la base de datos. Si la columna del conjunto de resultados es el resultado de una función, PDOStatement::getColumnMeta no devuelve este valor.|  
|flags|Especifica las marcas establecidas para esta columna. Siempre es 0.|  
|name|Especifica el nombre de la columna de la base de datos.|  
|mesa|Especifica el nombre de la tabla que contiene la columna de la base de datos. Siempre en blanco.|  
|len|Especifica la longitud de columnas.|  
|Precisión|Especifica la precisión numérica de esta columna.|  
|pdo_type|Especifica el tipo de esta columna tal y como representan las constantes de PDO::PARAM_*. Siempre es PDO::PARAM_STR (2).|  
  
En la versión 2.0 de los [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], se agregó compatibilidad con PDO.  
  
## <a name="example"></a>Ejemplo  
  
```  
<?php  
$database = "AdventureWorks";  
$server = "(local)";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$stmt = $conn->query("select * from Person.ContactType");  
$metadata = $stmt->getColumnMeta(2);  
var_dump($metadata);  
  
print $metadata['sqlsrv:decl_type'] . "\n";  
print $metadata['native_type'] . "\n";  
print $metadata['name'];  
?>  
```  
  
## <a name="sensitivity-data-classification-metadata"></a>Metadatos de clasificación de datos confidenciales

A partir de la versión 5.8.0, hay un nuevo atributo de instrucción `PDO::SQLSRV_ATTR_DATA_CLASSIFICATION` disponible para que los usuarios tengan acceso a los [metadatos de clasificación de datos confidenciales](../../relational-databases/security/sql-data-discovery-and-classification.md?tabs=t-sql#subheading-4) en Microsoft SQL Server 2019 con `PDOStatement::getColumnMeta`, que requiere Microsoft ODBC Driver 17.4.2 o superior.

Tenga en cuenta que el atributo `PDO::SQLSRV_ATTR_DATA_CLASSIFICATION` es `false` de forma predeterminada, pero cuando se establece en `true`, el campo de matriz mencionado, `flags`, se rellenará con los metadatos de clasificación de datos confidenciales, si existen. 

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

Para acceder a los metadatos, use `PDOStatement::getColumnMeta` después de establecer `PDO::SQLSRV_ATTR_DATA_CLASSIFICATION` en true, tal como se muestra en el fragmento de código siguiente:

```
$options = array(PDO::SQLSRV_ATTR_DATA_CLASSIFICATION => true);
$tableName = 'Patients';
$tsql = "SELECT * FROM $tableName";
$stmt = $conn->prepare($tsql, $options);
$stmt->execute();
$numCol = $stmt->columnCount();

for ($i = 0; $i < $numCol; $i++) {
    $metadata = $stmt->getColumnMeta($i);
    $jstr = json_encode($metadata);
    echo $jstr . PHP_EOL;
}
```

La salida de los metadatos de todas las columnas es:

```
{"flags":{"Data Classification":[]},"sqlsrv:decl_type":"int identity","native_type":"string","table":"","pdo_type":2,"name":"PatientId","len":10,"precision":0}
{"flags":{"Data Classification":[{"Label":{"name":"Highly Confidential - secure privacy","id":""},"Information Type":{"name":"Credentials","id":""}}]},"sqlsrv:decl_type":"char","native_type":"string","table":"","pdo_type":2,"name":"SSN","len":11,"precision":0}
{"flags":{"Data Classification":[]},"sqlsrv:decl_type":"nvarchar","native_type":"string","table":"","pdo_type":2,"name":"FirstName","len":50,"precision":0}
{"flags":{"Data Classification":[]},"sqlsrv:decl_type":"nvarchar","native_type":"string","table":"","pdo_type":2,"name":"LastName","len":50,"precision":0}
{"flags":{"Data Classification":[{"Label":{"name":"Confidential Personal Data","id":""},"Information Type":{"name":"Birthdays","id":""}}]},"sqlsrv:decl_type":"date","native_type":"string","table":"","pdo_type":2,"name":"BirthDate","len":10,"precision":0}
```

Si modificamos el fragmento de código anterior estableciendo `PDO::SQLSRV_ATTR_DATA_CLASSIFICATION` en `false` (caso predeterminado), el campo `flags` siempre será `0` como antes, como se indica a continuación:

```
{"flags":0,"sqlsrv:decl_type":"int identity","native_type":"string","table":"","pdo_type":2,"name":"PatientId","len":10,"precision":0}
{"flags":0,"sqlsrv:decl_type":"char","native_type":"string","table":"","pdo_type":2,"name":"SSN","len":11,"precision":0}
{"flags":0,"sqlsrv:decl_type":"nvarchar","native_type":"string","table":"","pdo_type":2,"name":"FirstName","len":50,"precision":0}
{"flags":0,"sqlsrv:decl_type":"nvarchar","native_type":"string","table":"","pdo_type":2,"name":"LastName","len":50,"precision":0}
{"flags":0,"sqlsrv:decl_type":"date","native_type":"string","table":"","pdo_type":2,"name":"BirthDate","len":10,"precision":0}
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
array(1) {
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
[Clase PDOStatement](../../connect/php/pdostatement-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
