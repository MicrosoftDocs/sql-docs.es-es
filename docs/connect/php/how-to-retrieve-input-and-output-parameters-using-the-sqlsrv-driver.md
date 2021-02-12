---
title: Procedimientos para recuperar los parámetros de E/S con el controlador SQLSRV
description: En este tema se describe cómo recuperar los parámetros de entrada y salida mediante procedimientos almacenados y el controlador SQLSRV de Microsoft para PHP en SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- stored procedure support
ms.assetid: 9a7c5f60-67f9-4968-a3a8-c256ee481da2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 33eb988309b6105818e150010dd44627efaced4b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046139"
---
# <a name="how-to-retrieve-input-and-output-parameters-using-the-sqlsrv-driver"></a>Procedimientos: Recuperación de los parámetros de entrada y salida con el controlador SQLSRV
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

En este tema se muestra cómo usar el controlador SQLSRV para llamar a un procedimiento almacenado en el que se ha definido un parámetro como parámetro de entrada/salida, y cómo recuperar los resultados. Cuando se recupera un parámetro de salida o uno de entrada/salida, se deben usar todos los resultados que devuelve el procedimiento almacenado antes de que pueda accederse al valor del parámetro devuelto.  
  
> [!NOTE]  
>  Las variables que se inicializan o actualizan a **Null**, **DateTime** o tipos de secuencia no se pueden usar como parámetros de salida.  
  
## <a name="example-1"></a>Ejemplo 1
En el ejemplo siguiente se llama un procedimiento almacenado que resta las horas de vacaciones utilizadas de las horas de vacaciones disponibles de un empleado determinado. La variable que representa las horas de vacaciones utilizadas, *$vacationHrs*, se transmite al procedimiento almacenado como un parámetro de entrada. Después de actualizar las horas de vacaciones disponibles, el procedimiento almacenado usa el mismo parámetro para devolver el número de horas de vacaciones restantes.  
  
> [!NOTE]  
> Al inicializar *$vacationHrs* en 4, el valor del tipo PHPTYPE devuelto se establece como un entero. Para garantizar la integridad del tipo de datos, los parámetros de entrada/salida se deben inicializar antes de llamar al procedimiento almacenado, o bien hay que especificar el tipo PHPTYPE deseado. Para obtener información sobre cómo especificar el tipo PHPTYPE, consulte [How to: Specify PHP Data Types](../../connect/php/how-to-specify-php-data-types.md).  
  
Dado que el procedimiento almacenado devuelve dos resultados, se debe llamar a [sqlsrv_next_result](../../connect/php/sqlsrv-next-result.md) una vez que se haya ejecutado el procedimiento almacenado para que esté disponible el valor del parámetro de salida. Después de llamar a **sqlsrv_next_result**, *$vacationHrs* contiene el valor del parámetro de salida devuelto por el procedimiento almacenado.  
  
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
$tsql_dropSP = "IF OBJECT_ID('SubtractVacationHours', 'P') IS NOT NULL  
                DROP PROCEDURE SubtractVacationHours";  
$stmt1 = sqlsrv_query( $conn, $tsql_dropSP);  
if( $stmt1 === false )  
{  
     echo "Error in executing statement 1.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Create the stored procedure. */  
$tsql_createSP = "CREATE PROCEDURE SubtractVacationHours  
                        @EmployeeID int,  
                        @VacationHrs smallint OUTPUT  
                  AS  
                  UPDATE HumanResources.Employee  
                  SET VacationHours = VacationHours - @VacationHrs  
                  WHERE EmployeeID = @EmployeeID;  
                  SET @VacationHrs = (SELECT VacationHours  
                                      FROM HumanResources.Employee  
                                      WHERE EmployeeID = @EmployeeID)";  
  
$stmt2 = sqlsrv_query( $conn, $tsql_createSP);  
if( $stmt2 === false )  
{  
     echo "Error in executing statement 2.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/*--------- The next few steps call the stored procedure. ---------*/  
  
/* Define the Transact-SQL query. Use question marks (?) in place of  
the parameters to be passed to the stored procedure */  
$tsql_callSP = "{call SubtractVacationHours( ?, ?)}";  
  
/* Define the parameter array. By default, the first parameter is an  
INPUT parameter. The second parameter is specified as an INOUT  
parameter. Initializing $vacationHrs to 8 sets the returned PHPTYPE to  
integer. To ensure data type integrity, output parameters should be  
initialized before calling the stored procedure, or the desired  
PHPTYPE should be specified in the $params array.*/  
  
$employeeId = 4;  
$vacationHrs = 8;  
$params = array(   
                 array($employeeId, SQLSRV_PARAM_IN),  
                 array(&$vacationHrs, SQLSRV_PARAM_INOUT)  
               );  
  
/* Execute the query. */  
$stmt3 = sqlsrv_query( $conn, $tsql_callSP, $params);  
if( $stmt3 === false )  
{  
     echo "Error in executing statement 3.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Display the value of the output parameter $vacationHrs. */  
sqlsrv_next_result($stmt3);  
echo "Remaining vacation hours: ".$vacationHrs;  
  
/*Free the statement and connection resources. */  
sqlsrv_free_stmt( $stmt1);  
sqlsrv_free_stmt( $stmt2);  
sqlsrv_free_stmt( $stmt3);  
sqlsrv_close( $conn);  
?>  
```  

> [!NOTE]
> Al enlazar un parámetro de entrada o salida a un tipo bigint, si el valor puede acabar fuera del intervalo de un [entero](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md), deberá especificar su tipo de campo SQL como SQLSRV_SQLTYPE_BIGINT. De lo contrario, puede dar como resultado una excepción "valor fuera del intervalo".

## <a name="example-2"></a>Ejemplo 2
En este ejemplo de código se muestra cómo enlazar un valor bigint largo como un parámetro de entrada o salida.  

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
$stmt = sqlsrv_prepare($conn, $outSql, array(array(&$bigintOut, SQLSRV_PARAM_INOUT, null, SQLSRV_SQLTYPE_BIGINT)));
sqlsrv_execute($stmt);
echo "$bigintOut\n";   // Expect 9223372036854

sqlsrv_free_stmt($stmt);  
sqlsrv_close($conn);  

?>
```

## <a name="see-also"></a>Consulte también  
[Procedimientos: Especificación de la dirección del parámetro con el controlador SQLSRV](../../connect/php/how-to-specify-parameter-direction-using-the-sqlsrv-driver.md)

[Procedimientos: Recuperación de los parámetros de salida con el controlador SQLSRV](../../connect/php/how-to-retrieve-output-parameters-using-the-sqlsrv-driver.md)

[Recuperación de datos](../../connect/php/retrieving-data.md)  
  
