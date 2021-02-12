---
title: Recuperación de datos de caracteres como secuencia mediante el controlador SQLSRV
description: En este tema se describe cómo recuperar datos de caracteres como una secuencia cuando se usa el controlador SQLSRV de Microsoft para PHP en SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- retrieving data, as a character stream
- streaming data
ms.assetid: 3c0dbca4-abfc-4449-b133-66c819681840
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 82a766e3f593b9f6ed49c2606703af2cdf717794
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038435"
---
# <a name="how-to-retrieve-character-data-as-a-stream-using-the-sqlsrv-driver"></a>Procedimientos: Recuperación de datos de caracteres como secuencia mediante el controlador SQLSRV
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

La recuperación de datos como un flujo solo está disponible en el controlador SQLSRV de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], pero no en el controlador PDO_SQLSRV.  
  
El controlador SQLSRV aprovecha las ventajas de las secuencias de PHP para recuperar grandes cantidades de datos del servidor. En el ejemplo de este tema se muestra cómo recuperar datos de caracteres como una secuencia.  
  
## <a name="example"></a>Ejemplo  
En el ejemplo siguiente se recupera una fila de la tabla *Production.ProductReview* de la base de datos de AdventureWorks. El campo *Comments* de la fila devuelta se recupera como un flujo y se muestra mediante la función PHP [fpassthru](https://php.net/manual/function.fpassthru.php).  
  
El proceso de recuperación de los datos como una secuencia se realiza utilizando [sqlsrv_fetch](../../connect/php/sqlsrv-fetch.md) y [sqlsrv_get_field](../../connect/php/sqlsrv-get-field.md) con el tipo de valor devuelto como una secuencia de caracteres. El tipo de valor devuelto se especifica mediante la constante **SQLSRV_PHPTYPE_STREAM**. Para obtener información sobre las constantes **sqlsrv**, vea [Constantes &#40;controladores de Microsoft para PHP para SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).  
  
En el ejemplo se da por hecho que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y la base de datos [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) están instalados en el equipo local. Los resultados se agregan a la consola cuando se ejecuta el ejemplo en la línea de comandos.  
  
```  
<?php  
/*Connect to the local server using Windows Authentication and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
if( $conn === false )  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Set up the Transact-SQL query. */  
$tsql = "SELECT ReviewerName,   
               CONVERT(varchar(32), ReviewDate, 107) AS [ReviewDate],  
               Rating,   
               Comments   
         FROM Production.ProductReview   
         WHERE ProductReviewID = ? ";  
  
/* Set the parameter value. */  
$productReviewID = 1;  
$params = array( $productReviewID);  
  
/* Execute the query. */  
$stmt = sqlsrv_query($conn, $tsql, $params);  
if( $stmt === false )  
{  
     echo "Error in statement execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Retrieve and display the data. The first three fields are retrieved  
as strings and the fourth as a stream with character encoding. */  
if(sqlsrv_fetch( $stmt ) === false )  
{  
     echo "Error in retrieving row.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
echo "Name: ".sqlsrv_get_field( $stmt, 0 )."\n";  
echo "Date: ".sqlsrv_get_field( $stmt, 1 )."\n";  
echo "Rating: ".sqlsrv_get_field( $stmt, 2 )."\n";  
echo "Comments: ";  
$comments = sqlsrv_get_field( $stmt, 3,   
                             SQLSRV_PHPTYPE_STREAM(SQLSRV_ENC_CHAR));  
fpassthru($comments);  
  
/* Free the statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
Como en los tres primeros campos no se ha especificado el tipo de datos PHP devuelto, los campos se devuelven según el tipo de datos PHP predeterminado. Para obtener información sobre los tipos de datos PHP predeterminados, consulte [Default PHP Data Types](../../connect/php/default-php-data-types.md). Para obtener información sobre cómo especificar los tipos de valor devueltos PHP, consulte [How to: Specify PHP Data Types](../../connect/php/how-to-specify-php-data-types.md).  
  
## <a name="see-also"></a>Consulte también  
[Recuperación de datos](../../connect/php/retrieving-data.md)

[Recuperación de datos como una secuencia con el controlador SQLSRV](../../connect/php/retrieving-data-as-a-stream-using-the-sqlsrv-driver.md)

[Sobre los ejemplos de código de la documentación](../../connect/php/about-code-examples-in-the-documentation.md)  
  
