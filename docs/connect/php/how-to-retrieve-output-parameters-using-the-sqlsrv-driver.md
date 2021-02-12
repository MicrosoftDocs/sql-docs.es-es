---
title: 'Procedimientos: Recuperación de los parámetros de salida con el controlador SQLSRV'
description: Obtenga información sobre cómo usar y recuperar parámetros de salida en un procedimiento almacenado con el controlador SQLSRV de Microsoft para PHP para SQL Server.
ms.custom: ''
ms.date: 04/11/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- stored procedure support
ms.assetid: 1157bab7-6ad1-4bdb-a81c-662eea3e7fcd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6f0e165c04e303a5cb65046b013df8814bfbf515
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038375"
---
# <a name="how-to-retrieve-output-parameters-using-the-sqlsrv-driver"></a>Procedimientos: Recuperación de los parámetros de salida con el controlador SQLSRV
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

En este tema se muestra cómo llamar a un procedimiento almacenado en el que se ha definido un parámetro como parámetro de salida. Cuando se recupera un parámetro de salida o uno de entrada/salida, se deben usar todos los resultados que devuelve el procedimiento almacenado antes de que pueda accederse al valor del parámetro devuelto.  
  
> [!NOTE]  
>  Las variables que se inicializan o actualizan a **Null**, **DateTime** o tipos de secuencia no se pueden usar como parámetros de salida.  
  
Puede producirse un truncamiento de datos cuando se utilizan tipos de secuencia, por ejemplo, SQLSRV_SQLTYPE_VARCHAR('max'), como parámetros de salida. No se permiten tipos de secuencia como parámetros de salida. En cuanto a los tipos que no son de secuencia, puede producirse un truncamiento de datos si no se especifica la longitud del parámetro de salida o si la longitud especificada no es suficientemente grande para el parámetro de salida.  
  
## <a name="example-1"></a>Ejemplo 1
En el ejemplo siguiente se llama a un procedimiento almacenado que devuelve las ventas hasta la fecha que ha realizado un determinado empleado. La variable PHP *$lastName* es un parámetro de entrada y *$salesYTD* es un parámetro de salida.  
  
> [!NOTE]  
> Al inicializar *$salesYTD* en 0,0, el valor del tipo PHPTYPE devuelto se establece en **float**. Para garantizar la integridad del tipo de datos, los parámetros de salida se deben inicializar antes de llamar al procedimiento almacenado, o bien hay que especificar el tipo PHPTYPE deseado. Para obtener información sobre cómo especificar el tipo PHPTYPE, consulte [How to: Specify PHP Data Types](../../connect/php/how-to-specify-php-data-types.md).  
  
Dado que el procedimiento almacenado solo devuelve un resultado, *$salesYTD* contiene el valor devuelto del parámetro de salida inmediatamente después de ejecutar el procedimiento almacenado.  
  
> [!NOTE]  
> Se recomienda llamar a procedimientos almacenados mediante sintaxis canónica. Para obtener más información sobre la sintaxis canónica, vea [Llamar a un procedimiento almacenado](../../relational-databases/native-client-odbc-stored-procedures/calling-a-stored-procedure.md).  
  
En el ejemplo se da por hecho que SQL Server y la base de datos [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) están instalados en el equipo local. Los resultados se agregan a la consola cuando se ejecuta el ejemplo en la línea de comandos.  
  
```  
<?php  
/* Connect to the local server using Windows Authentication and   
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
if( $conn === false )  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Drop the stored procedure if it already exists. */  
$tsql_dropSP = "IF OBJECT_ID('GetEmployeeSalesYTD', 'P') IS NOT NULL  
                DROP PROCEDURE GetEmployeeSalesYTD";  
$stmt1 = sqlsrv_query( $conn, $tsql_dropSP);  
if( $stmt1 === false )  
{  
     echo "Error in executing statement 1.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Create the stored procedure. */  
$tsql_createSP = " CREATE PROCEDURE GetEmployeeSalesYTD  
                   @SalesPerson nvarchar(50),  
                   @SalesYTD money OUTPUT  
                   AS  
                   SELECT @SalesYTD = SalesYTD  
                   FROM Sales.SalesPerson AS sp  
                   JOIN HumanResources.vEmployee AS e   
                   ON e.EmployeeID = sp.SalesPersonID  
                   WHERE LastName = @SalesPerson";  
$stmt2 = sqlsrv_query( $conn, $tsql_createSP);  
if( $stmt2 === false )  
{  
     echo "Error in executing statement 2.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/*--------- The next few steps call the stored procedure. ---------*/  
  
/* Define the Transact-SQL query. Use question marks (?) in place of  
 the parameters to be passed to the stored procedure */  
$tsql_callSP = "{call GetEmployeeSalesYTD( ?, ? )}";  
  
/* Define the parameter array. By default, the first parameter is an  
INPUT parameter. The second parameter is specified as an OUTPUT  
parameter. Initializing $salesYTD to 0.0 sets the returned PHPTYPE to  
float. To ensure data type integrity, output parameters should be  
initialized before calling the stored procedure, or the desired  
PHPTYPE should be specified in the $params array.*/  
$lastName = "Blythe";  
$salesYTD = 0.0;  
$params = array(   
                 array($lastName, SQLSRV_PARAM_IN),  
                 array(&$salesYTD, SQLSRV_PARAM_OUT)  
               );  
  
/* Execute the query. */  
$stmt3 = sqlsrv_query( $conn, $tsql_callSP, $params);  
if( $stmt3 === false )  
{  
     echo "Error in executing statement 3.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Display the value of the output parameter $salesYTD. */  
echo "YTD sales for ".$lastName." are ". $salesYTD. ".";  
  
/*Free the statement and connection resources. */  
sqlsrv_free_stmt( $stmt1);  
sqlsrv_free_stmt( $stmt2);  
sqlsrv_free_stmt( $stmt3);  
sqlsrv_close( $conn);  
?>  
```  

> [!NOTE]
> Al enlazar un parámetro de salida a un tipo bigint, si el valor puede acabar fuera del intervalo de un [entero](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md), deberá especificar su tipo de campo SQL como SQLSRV_SQLTYPE_BIGINT. De lo contrario, puede dar como resultado una excepción "valor fuera del intervalo".

## <a name="example-2"></a>Ejemplo 2
En este ejemplo de código se muestra cómo enlazar un valor bigint largo como un parámetro de salida.  

```
<?php
$serverName = "(local)";
$connectionInfo = array("Database"=>"testDB");  
$conn = sqlsrv_connect($serverName, $connectionInfo);  
if ($conn === false) {  
    echo "Could not connect.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  

// Assume the stored procedure spTestProcedure exists, which retrieves a bigint value of some large number
// e.g. 9223372036854
$bigintOut = 0;
$outSql = "{CALL spTestProcedure (?)}";
$stmt = sqlsrv_prepare($conn, $outSql, array(array(&$bigintOut, SQLSRV_PARAM_OUT, null, SQLSRV_SQLTYPE_BIGINT)));
sqlsrv_execute($stmt);
echo "$bigintOut\n";   // Expect 9223372036854

sqlsrv_free_stmt($stmt);  
sqlsrv_close($conn);  

?>
```

## <a name="see-also"></a>Consulte también  
[Procedimientos: Especificación de la dirección del parámetro con el controlador SQLSRV](../../connect/php/how-to-specify-parameter-direction-using-the-sqlsrv-driver.md)

[Procedimientos: Recuperación de los parámetros de entrada y salida con el controlador SQLSRV](../../connect/php/how-to-retrieve-input-and-output-parameters-using-the-sqlsrv-driver.md)

[Recuperación de datos](../../connect/php/retrieving-data.md)  
  
