---
title: sqlsrv_query
description: La función sqlsrv_query proporciona un método para ejecutar una consulta con una cantidad mínima de código y se puede usar para ejecutar consultas con parámetros.
ms.custom: ''
ms.date: 04/11/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_query
apitype: NA
helpviewer_keywords:
- sqlsrv_query
- executing queries
- API Reference, sqlsrv_query
ms.assetid: 9fa7c4c8-4da8-4299-9893-f61815055aa3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 751fd08e63cccfe4293785191a223728690ed13e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201076"
---
# <a name="sqlsrv_query"></a>sqlsrv_query
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Prepara y ejecuta una instrucción.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sqlsrv_query(resource $conn, string $tsql [, array $params [, array $options]])  
```  
  
#### <a name="parameters"></a>Parámetros  
*$conn*: el recurso de conexión asociado a la instrucción preparada.  
  
*$tsql*: la expresión Transact-SQL que corresponde a la instrucción preparada.  
  
*$params* [OPCIONAL]: **matriz** de valores que se corresponden con parámetros de una consulta con parámetros. Cada elemento de la matriz puede ser uno de los siguientes:
  
-   Un valor literal  
  
-   Una variable PHP  
  
-   Una **matriz** con la siguiente estructura:  
  
    ```  
    array($value [, $direction [, $phpType [, $sqlType]]])  
    ```  
  
    En la siguiente tabla se incluye la descripción de cada elemento de la matriz:  
  
    |Elemento|Descripción|  
    |-----------|---------------|  
    |*$value*|Un valor literal, una variable PHP o una variable por referencia PHP.|  
    |*$direction*[OPCIONAL]|Una de las siguientes constantes **SQLSRV_PARAM_\* *_ usadas para indicar la dirección del parámetro: _* SQLSRV_PARAM_IN**, **SQLSRV_PARAM_OUT**, **SQLSRV_PARAM_INOUT**. El valor predeterminado es **SQLSRV_PARAM_IN**.<br /><br />Para obtener más información sobre las constantes de PHP, vea [Constantes &#40;controladores de Microsoft para PHP para SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).|  
    |*$phpType*[OPCIONAL]|Constante **SQLSRV_PHPTYPE_\** _ que especifica el tipo de datos PHP del valor devuelto.<br /><br />Para obtener más información sobre las constantes de PHP, vea [Constantes &#40;controladores de Microsoft para PHP para SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).|  
    |_$sqlType* [OPCIONAL]|Constante **SQLSRV_SQLTYPE_\** _ que especifica el tipo de datos de SQL Server del valor de entrada.<br /><br />Para obtener más información sobre las constantes de PHP, vea [Constantes &#40;controladores de Microsoft para PHP para SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).|  
  
_$options* [OPCIONAL]: una matriz asociativa que establece propiedades de consultas. Es la misma lista de claves que también admite [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md#properties).
  
## <a name="return-value"></a>Valor devuelto  
Un recurso de instrucción. Si no se puede crear o ejecutar la instrucción, se devuelve **false**.  
  
## <a name="remarks"></a>Observaciones  
La función **sqlsrv_query** resulta apropiada para consultas únicas y debe establecerse como la opción predeterminada para ejecutar consultas, a menos que se apliquen circunstancias especiales. Esta función proporciona un método simplificado para ejecutar una consulta con una cantidad mínima de código. La función **sqlsrv_query** realiza los procesos de preparación y ejecución de la instrucción, y se puede usar para ejecutar consultas con parámetros.  
  
Para más información, vea: [Cómo: Recuperación de los parámetros de salida con el controlador SQLSRV](../../connect/php/how-to-retrieve-output-parameters-using-the-sqlsrv-driver.md).  
  
## <a name="example-1"></a>Ejemplo 1  
En el siguiente ejemplo, se inserta una fila única en la tabla *Sales.SalesOrderDetail* de la base de datos de AdventureWorks. En el ejemplo se da por hecho que SQL Server y la base de datos [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) están instalados en el equipo local. Los resultados se agregan a la consola cuando se ejecuta el ejemplo en la línea de comandos.  
  
> [!NOTE]  
> Aunque en el ejemplo siguiente se usa una instrucción INSERT con el objetivo de mostrar el uso de **sqlsrv_query** para la ejecución de una instrucción única, el concepto se aplica a cualquier instrucción de Transact-SQL.  
  
```  
<?php  
/* Connect to the local server using Windows Authentication and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array("Database"=>"AdventureWorks");  
$conn = sqlsrv_connect($serverName, $connectionInfo);  
if ($conn === false) {  
    echo "Could not connect.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Set up the parameterized query. */  
$tsql = "INSERT INTO Sales.SalesOrderDetail   
        (SalesOrderID,   
         OrderQty,   
         ProductID,   
         SpecialOfferID,   
         UnitPrice,   
         UnitPriceDiscount)  
        VALUES   
        (?, ?, ?, ?, ?, ?)";  
  
/* Set parameter values. */  
$params = array(75123, 5, 741, 1, 818.70, 0.00);  
  
/* Prepare and execute the query. */  
$stmt = sqlsrv_query($conn, $tsql, $params);  
if ($stmt) {  
    echo "Row successfully inserted.\n";  
} else {  
    echo "Row insertion failed.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt($stmt);  
sqlsrv_close($conn);  
?>  
```  
  
## <a name="example-2"></a>Ejemplo 2  
En el siguiente ejemplo se actualiza un campo de la tabla *Sales.SalesOrderDetail* de la base de datos AdventureWorks. En el ejemplo se da por hecho que SQL Server y la base de datos [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) están instalados en el equipo local. Los resultados se agregan a la consola cuando se ejecuta el ejemplo en la línea de comandos.  
  
```  
<?php  
/* Connect to the local server using Windows Authentication and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array("Database"=>"AdventureWorks");  
$conn = sqlsrv_connect($serverName, $connectionInfo);  
if ($conn === false) {  
    echo "Could not connect.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Set up the parameterized query. */  
$tsql = "UPDATE Sales.SalesOrderDetail   
         SET OrderQty = (?)   
         WHERE SalesOrderDetailID = (?)";  
  
/* Assign literal parameter values. */  
$params = array(5, 10);  
  
/* Execute the query. */  
if (sqlsrv_query($conn, $tsql, $params)) {  
    echo "Statement executed.\n";  
} else {  
    echo "Error in statement execution.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Free connection resources. */  
sqlsrv_close($conn);  
?>  
```  
  
> [!NOTE]
> Se recomienda utilizar cadenas como entradas cuando se vinculen valores a una [columna decimal o numérica](../../t-sql/data-types/decimal-and-numeric-transact-sql.md) para garantizar la precisión y la exactitud, ya que PHP tiene una precisión limitada para [números de punto flotante](https://php.net/manual/en/language.types.float.php). Lo mismo se aplica a las columnas de tipo bigint, especialmente cuando los valores están fuera del intervalo de un [entero](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md).

## <a name="example-3"></a>Ejemplo 3  
Este ejemplo de código muestra cómo enlazar un valor decimal como un parámetro de entrada.  

```
<?php
$serverName = "(local)";
$connectionInfo = array("Database"=>"YourTestDB");  
$conn = sqlsrv_connect($serverName, $connectionInfo);  
if ($conn === false) {  
     echo "Could not connect.\n";  
     die(print_r(sqlsrv_errors(), true));  
}  

// Assume TestTable exists with a decimal field 
$input = "9223372036854.80000";
$params = array($input);
$stmt = sqlsrv_query($conn, "INSERT INTO TestTable (DecimalCol) VALUES (?)", $params);

sqlsrv_free_stmt($stmt);  
sqlsrv_close($conn);  

?>
```

## <a name="example-4"></a>Ejemplo 4
Este ejemplo de código muestra cómo crear una tabla de tipos [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) y capturar los datos insertados.

```
<?php
$server = 'serverName';
$dbName = 'databaseName';
$uid = 'yourUserName';
$pwd = 'yourPassword';

$options = array("Database"=>$dbName, "UID"=>$uid, "PWD"=>$pwd);
$conn = sqlsrv_connect($server, $options);
if($conn === false) {
    die(print_r(sqlsrv_errors(), true));
}

$tableName = 'testTable';
$query = "CREATE TABLE $tableName ([c1_int] sql_variant, [c2_varchar] sql_variant)";

$stmt = sqlsrv_query($conn, $query);
if($stmt === false) {
    die(print_r(sqlsrv_errors(), true));
}
sqlsrv_free_stmt($stmt);

$query = "INSERT INTO [$tableName] (c1_int, c2_varchar) VALUES (1, 'test_data')";
$stmt = sqlsrv_query($conn, $query);
if($stmt === false) {
    die(print_r(sqlsrv_errors(), true));
}
sqlsrv_free_stmt($stmt);

$query = "SELECT * FROM $tableName";
$stmt = sqlsrv_query($conn, $query);

if(sqlsrv_fetch($stmt) === false) {
    die(print_r(sqlsrv_errors(), true));
}

$col1 = sqlsrv_get_field($stmt, 0);
echo "First field:  $col1 \n";

$col2 = sqlsrv_get_field($stmt, 1);
echo "Second field:  $col2 \n";

sqlsrv_free_stmt($stmt);
sqlsrv_close($conn);

?>
```

El resultado esperado sería:

```
First field:  1
Second field:  test_data
```

## <a name="see-also"></a>Consulte también  
[Referencia de API del controlador SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  

[Cómo: Realización de consultas con parámetros](../../connect/php/how-to-perform-parameterized-queries.md)  

[Sobre los ejemplos de código de la documentación](../../connect/php/about-code-examples-in-the-documentation.md)  

[Cómo: Envío de datos en forma de secuencia](../../connect/php/how-to-send-data-as-a-stream.md)  

[Uso de parámetros direccionales](../../connect/php/using-directional-parameters.md)  

