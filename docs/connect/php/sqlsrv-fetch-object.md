---
description: sqlsrv_fetch_object
title: sqlsrv_fetch_object | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_fetch_object
apitype: NA
helpviewer_keywords:
- sqlsrv_fetch_object
- API Reference, sqlsrv_fetch_object
- retrieving data, as an object
ms.assetid: 4ce2df2c-083a-4a4d-a1e2-e866e63707d5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6b173c2676f25b4866e1981611c1375e4cd1bf6b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99154466"
---
# <a name="sqlsrv_fetch_object"></a>sqlsrv_fetch_object
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Recupera la fila siguiente de datos como un objeto PHP.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sqlsrv_fetch_object( resource $stmt [, string $className [, array $ctorParams[, row[, ]offset]]])  
```  
  
#### <a name="parameters"></a>Parámetros  
*$stmt*: un recurso de instrucción correspondiente a una instrucción ejecutada.  
  
*$className* (opcional): una cadena que especifica el nombre de la clase para crear una instancia. Si un valor del parámetro *$className* no se especifica, se creará una instancia PHP de **stdClass** .  
  
*$ctorParams* (opcional): una matriz que contiene los valores que se transmiten al constructor de la clase especificada con el parámetro *$className*. Si el constructor de la clase especificada acepta los valores de parámetro, el parámetro *$ctorParams* debe usarse al llamar a **sqlsrv_fetch_object**.  
  
*row* [OPCIONAL]: Uno de los siguientes valores; sirve para especificar la fila a la que se accederá en un conjunto de resultados que utilice un cursor desplazable (Si se indica *row*, se tendrá que especificar explícitamente *$className* y *$ctorParams*, aunque debe proporcionar el valor NULL para *$className* y *$ctorParams*).  
  
-   SQLSRV_SCROLL_NEXT  
  
-   SQLSRV_SCROLL_PRIOR  
  
-   SQLSRV_SCROLL_FIRST  
  
-   SQLSRV_SCROLL_LAST  
  
-   SQLSRV_SCROLL_ABSOLUTE  
  
-   SQLSRV_SCROLL_RELATIVE  
  
Para obtener más información sobre estos valores, vea [Especificación de un tipo de cursor y selección de filas](../../connect/php/specifying-a-cursor-type-and-selecting-rows.md).  
  
*offset* [OPCIONAL]: Se usa con SQLSRV_SCROLL_ABSOLUTE y SQLSRV_SCROLL_RELATIVE para especificar la fila que se va a recuperar. El primer registro del conjunto de resultados es 0.  
  
## <a name="return-value"></a>Valor devuelto  
Un objeto PHP con propiedades que se corresponden con los nombres de campo del conjunto de resultados. Los valores de propiedad se rellenan con los valores de campo del conjunto de resultados correspondiente. Si la clase especificada con el parámetro *$className* opcional no existe o si no hay ningún conjunto de resultados activo asociado con la instrucción especificada, se devolverá el valor **False** . Si no hay más filas para recuperar, se devolverá el valor **Null** .  
  
El tipo de datos de un valor en el objeto devuelto será el tipo de datos PHP predeterminado. Para obtener información sobre los tipos de datos PHP predeterminados, consulte [Default PHP Data Types](../../connect/php/default-php-data-types.md).  
  
## <a name="remarks"></a>Observaciones  
Si se especifica un nombre de clase con el parámetro opcional *$className* , se crea una instancia en un objeto de este tipo de clase. Si la clase tiene propiedades cuyos nombres coinciden con los nombres de campo del conjunto de resultados, los valores correspondientes del conjunto de resultados se aplican a las propiedades. Si un nombre de campo del conjunto de resultados no coincide con una propiedad de clase, se agrega una propiedad con el nombre de campo del conjunto de resultados al objeto y el valor del conjunto de resultados se aplica a la propiedad.  
  
Las reglas siguientes se aplican cuando se especifica una clase con el parámetro *$className* :  
  
-   La coincidencia distingue entre mayúsculas y minúsculas. Por ejemplo, el nombre de propiedad CustomerId no coincide con el nombre del campo CustomerID. En este caso, se agregaría una propiedad CustomerID al objeto y el valor del campo CustomerID se otorgaría a la propiedad CustomerID.  
  
-   La coincidencia se produce independientemente de los modificadores de acceso. Por ejemplo, si la clase especificada tiene una propiedad privada cuyo nombre coincide con un nombre de campo del conjunto de resultados, el valor del campo de conjunto de resultados se aplica a la propiedad.  
  
-   Se omiten los tipos de datos de propiedad de clase. Si el campo "CustomerID" del conjunto de resultados es una cadena, pero la propiedad "CustomerID" de la clase es un valor entero, el valor de cadena del conjunto de resultados se crea en la propiedad "CustomerID".  
  
-   Si la clase especificada no existe, la función devuelve **False** y agrega un error a la colección de errores. Para obtener información sobre cómo recuperar información de errores, consulte [sqlsrv_errors](../../connect/php/sqlsrv-errors.md).  
  
Si se devuelve un campo sin nombre, **sqlsrv_fetch_object** descartará el valor del campo y generará una advertencia. Por ejemplo, observe esta instrucción de Transact-SQL que inserta un valor en una tabla de base de datos y recupera la clave principal generada por el servidor:  
  
```sql
INSERT INTO Production.ProductPhoto (LargePhoto) VALUES (?);  
SELECT SCOPE_IDENTITY()
```
  
Si los resultados que devuelve esta consulta se recuperan con **sqlsrv_fetch_object**, el valor que devuelve `SELECT SCOPE_IDENTITY()` se descartará y se generará una advertencia. Para evitarlo, puede especificar un nombre para el campo devuelto en la instrucción de Transact-SQL. A continuación, se muestra una forma de especificar un nombre de columna en Transact-SQL:  
  
```sql
SELECT SCOPE_IDENTITY() AS PictureID
```
  
## <a name="object-example"></a>Ejemplo de objeto  
En el siguiente ejemplo se recupera cada fila de un conjunto de resultados como un objeto PHP. En el ejemplo se da por hecho que SQL Server y la base de datos [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) están instalados en el equipo local. Los resultados se agregan a la consola cuando se ejecuta el ejemplo en la línea de comandos.  
  
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
  
/* Set up and execute the query. */  
$tsql = "SELECT FirstName, LastName  
         FROM Person.Contact  
         WHERE LastName='Alan'";  
$stmt = sqlsrv_query( $conn, $tsql);  
if( $stmt === false )  
{  
     echo "Error in query preparation/execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Retrieve each row as a PHP object and display the results.*/  
while( $obj = sqlsrv_fetch_object( $stmt))  
{  
      echo $obj->LastName.", ".$obj->FirstName."\n";  
}  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="class-example"></a>Ejemplo de clase  
En el ejemplo siguiente se recupera cada fila de un conjunto de resultados como una instancia de la clase *Product* definida en la secuencia de comandos. En el ejemplo se recupera información del producto de las tablas *Purchasing.PurchaseOrderDetail* y *Production.Product* de la base de datos AdventureWorks para productos que tienen una fecha de vencimiento especificada (*DueDate*) y una cantidad mantenida en existencias (*StockQty*) inferior a un valor determinado. En el ejemplo se resaltan algunas de las reglas que se aplican cuando se especifica una clase en una llamada a **sqlsrv_fetch_object**:  
  
-   La variable *$product* es una instancia de la clase *Product* , ya que se especificó "Product" con el parámetro *$className* y existe la clase *Product* .  
  
-   La propiedad *Name* se agrega a la instancia *$product* porque no coincide con la propiedad existente *name* .  
  
-   La propiedad *Color* se agrega a la instancia *$product* porque no hay ninguna propiedad coincidente.  
  
-   La propiedad privada *UnitPrice* se rellena con el valor del campo *UnitPrice* .  
  
En el ejemplo se da por hecho que SQL Server y la base de datos [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) están instalados en el equipo local. Los resultados se agregan a la consola cuando se ejecuta el ejemplo en la línea de comandos.  
  
```  
<?php  
/* Define the Product class. */  
class Product  
{  
     /* Constructor */  
     public function Product($ID)  
     {  
          $this->objID = $ID;  
     }  
     public $objID;  
     public $name;  
     public $StockedQty;  
     public $SafetyStockLevel;  
     private $UnitPrice;  
     function getPrice()  
     {  
          return $this->UnitPrice;  
     }  
}  
  
/* Connect to the local server using Windows Authentication, and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
if( $conn === false )  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Define the query. */  
$tsql = "SELECT Name,  
                SafetyStockLevel,  
                StockedQty,  
                UnitPrice,  
                Color  
         FROM Purchasing.PurchaseOrderDetail AS pdo  
         JOIN Production.Product AS p  
         ON pdo.ProductID = p.ProductID  
         WHERE pdo.StockedQty < ?  
         AND pdo.DueDate= ?";  
  
/* Set the parameter values. */  
$params = array(3, '2002-01-29');  
  
/* Execute the query. */  
$stmt = sqlsrv_query( $conn, $tsql, $params);  
if ( $stmt )  
{  
     echo "Statement executed.\n";  
}   
else   
{  
     echo "Error in statement execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Iterate through the result set, printing a row of data upon each  
 iteration. Note the following:  
     1) $product is an instance of the Product class.  
     2) The $ctorParams parameter is required in the call to  
        sqlsrv_fetch_object, because the Product class constructor is  
        explicity defined and requires parameter values.  
     3) The "Name" property is added to the $product instance because  
        the existing "name" property does not match.  
     4) The "Color" property is added to the $product instance  
        because there is no matching property.  
     5) The private property "UnitPrice" is populated with the value  
        of the "UnitPrice" field.*/  
$i=0; //Used as the $objID in the Product class constructor.  
while( $product = sqlsrv_fetch_object( $stmt, "Product", array($i)))  
{  
     echo "Object ID: ".$product->objID."\n";  
     echo "Product Name: ".$product->Name."\n";  
     echo "Stocked Qty: ".$product->StockedQty."\n";  
     echo "Safety Stock Level: ".$product->SafetyStockLevel."\n";  
     echo "Product Color: ".$product->Color."\n";  
     echo "Unit Price: ".$product->getPrice()."\n";  
     echo "-----------------\n";  
     $i++;  
}  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
La variable **sqlsrv_fetch_object** siempre devuelve datos según los [Default PHP Data Types](../../connect/php/default-php-data-types.md). Para obtener información sobre cómo especificar el tipo de datos PHP, vea [Procedimiento para especificar tipos de datos PHP](../../connect/php/how-to-specify-php-data-types.md).  
  
Si se devuelve un campo sin nombre, **sqlsrv_fetch_object** descartará el valor del campo y generará una advertencia. Por ejemplo, observe esta instrucción de Transact-SQL que inserta un valor en una tabla de base de datos y recupera la clave principal generada por el servidor:  
  
```sql
INSERT INTO Production.ProductPhoto (LargePhoto) VALUES (?);  
SELECT SCOPE_IDENTITY()
```
  
Si los resultados que devuelve esta consulta se recuperan con **sqlsrv_fetch_object**, el valor que devuelve `SELECT SCOPE_IDENTITY()` se descartará y se generará una advertencia. Para evitarlo, puede especificar un nombre para el campo devuelto en la instrucción de Transact-SQL. A continuación, se muestra una forma de especificar un nombre de columna en Transact-SQL:  
  
```sql
SELECT SCOPE_IDENTITY() AS PictureID
```
  
## <a name="see-also"></a>Consulte también  
[Recuperación de datos](../../connect/php/retrieving-data.md)  

[Sobre los ejemplos de código de la documentación](../../connect/php/about-code-examples-in-the-documentation.md)  

[Referencia de API del controlador SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  
  
