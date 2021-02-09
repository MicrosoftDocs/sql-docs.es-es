---
title: PDO::errorInfo
description: Referencia de API de la función PDO::errorInfo en el controlador PDO_SQLSRV de Microsoft para PHP en SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 9d5481d5-13bc-4388-b3aa-78676c0fc709
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 46b57275d92f4eb6acb64c276e3b34ea67efb429
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202032"
---
# <a name="pdoerrorinfo"></a>PDO::errorInfo
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Recupera información ampliada de errores de la operación más reciente en el identificador de la base de datos.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
array PDO::errorInfo();  
```  
  
## <a name="return-value"></a>Valor devuelto  
Una matriz de información de errores de la operación más reciente en el identificador de la base de datos. La matriz consta de los siguientes campos:  
  
-   El código de error de SQLSTATE  
  
-   El código de error específico del controlador  
  
-   El mensaje de error específico del controlador.  
  
Si no hay ningún error, o si no se establece el valor de SQLSTATE, los campos específicos del controlador serán NULL.  
  
## <a name="remarks"></a>Observaciones  
PDO::errorInfo solo recupera información de error para operaciones realizadas directamente en la base de datos. Utilice PDOStatement::errorInfo cuando se cree una instancia de PDOStatement con PDO::prepare o PDO::query.  
  
En la versión 2.0 de los [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], se agregó compatibilidad con PDO.  
  
## <a name="example"></a>Ejemplo  
En este ejemplo, no se ha escrito correctamente el nombre de la columna (`Cityx` en lugar de `City`), lo que ha provocado un error, del que se informa después.  
  
```php
<?php  
$conn = new PDO( "sqlsrv:server=(local) ; Database = AdventureWorks ", "");  
$query = "SELECT * FROM Person.Address where Cityx = 'Essen'";  
  
$conn->query($query);  
print $conn->errorCode();  
echo "\n";  
print_r ($conn->errorInfo());  
?>  
```  

## <a name="additional-odbc-messages"></a>Mensajes de ODBC adicionales

Cuando se produce una excepción, el controlador ODBC puede devolver más de un error para ayudar a diagnosticar problemas. Pero PDO::errorInfo siempre muestra solo el primer error. En respuesta a este [informe de errores](https://bugs.php.net/bug.php?id=78196), [PDO::errorInfo](https://www.php.net/manual/en/pdo.errorinfo.php) y [PDOStatement::errorInfo](https://www.php.net/manual/en/pdostatement.errorinfo.php) se han actualizado para indicar que los controladores deben mostrar *al menos* los tres campos siguientes:
```
0   SQLSTATE error code (a five characters alphanumeric identifier defined in the ANSI SQL standard).
1   Driver specific error code.
2   Driver specific error message.
```

A partir de la versión 5.9.0, el comportamiento predeterminado de PDO::errorInfo es mostrar los errores de ODBC adicionales, si están disponibles. Por ejemplo:

```php
<?php  
try {
    $conn = new PDO("sqlsrv:server=$server;", $uid, $pwd);
    $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    $stmt = $conn->prepare("SET NOCOUNT ON; USE $database; SELECT 1/0 AS col1");
    $stmt->execute();
} catch (PDOException $e) {
    var_dump($e->errorInfo);
}
?>  
```  

La ejecución del script anterior produce una excepción, y la salida debe ser similar a la siguiente:

```
array(6) {
  [0]=>
  string(5) "01000"
  [1]=>
  int(5701)
  [2]=>
  string(91) "[Microsoft][ODBC Driver 17 for SQL Server][SQL Server]Changed database context to 'tempdb'."
  [3]=>
  string(5) "22012"
  [4]=>
  int(8134)
  [5]=>
  string(87) "[Microsoft][ODBC Driver 17 for SQL Server][SQL Server]Divide by zero error encountered."
}
```

Si el usuario prefiere la manera anterior, puede usar una nueva opción de configuración `pdo_sqlsrv.report_additional_errors` para desactivarla. Simplemente agregue la siguiente línea al principio de cualquier script de PHP:

```
ini_set('pdo_sqlsrv.report_additional_errors', 0);
```

En este caso, al ejecutar el mismo script de ejemplo, la información de error que se mostrará será similar a la siguiente:

```
array(3) {
  [0]=>
  string(5) "01000"
  [1]=>
  int(5701)
  [2]=>
  string(91) "[Microsoft][ODBC Driver 17 for SQL Server][SQL Server]Changed database context to 'tempdb'."
}
```

Si es necesario, el usuario puede optar por agregar la siguiente línea al archivo php.ini para desactivar esta característica en todos sus scripts de PHP:

```
pdo_sqlsrv.report_additional_errors = 0
```

## <a name="warnings-and-errors"></a>Advertencias y errores

A partir de la versión 5.9.0, las advertencias de ODBC ya no se registran como errores. Es decir, los [códigos de error](https://docs.microsoft.com/sql/odbc/reference/appendixes/appendix-a-odbc-error-codes) con el prefijo "01" se registran como advertencias. En otras palabras, si el usuario quiere registrar solo los errores, debe actualizar el archivo php.ini de la siguiente manera:

```
[pdo_sqlsrv]  
pdo_sqlsrv.log_severity = 1
```

En este caso, el archivo de registro no contendrá ningún mensaje de advertencia. Consulte cómo funciona el [registro](https://docs.microsoft.com/sql/connect/php/logging-activity#logging-activity-using-the-pdo_sqlsrv-driver) para los usuarios que usan el controlador pdo_sqlsrv.

## <a name="see-also"></a>Consulte también  
[Clase PDO](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  

[PDOStatement::errorInfo](../../connect/php/pdostatement-errorinfo.md)
