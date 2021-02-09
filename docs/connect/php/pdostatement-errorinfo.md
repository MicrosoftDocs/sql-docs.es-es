---
title: PDOStatement::errorInfo
description: Referencia de API de la función PDOStatement::errorInfo en el controlador PDO_SQLSRV de Microsoft para PHP en SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: e45bebe8-ea4c-49b6-93db-cf1ae65f530c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0474b0c1225a248b47f0892ac940f2e58ab0efa0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206362"
---
# <a name="pdostatementerrorinfo"></a>PDOStatement::errorInfo
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Recupera información ampliada de errores de la operación más reciente en el identificador de instrucción.  
  
## <a name="syntax"></a>Sintaxis  

```
array PDOStatement::errorInfo();
```
  
## <a name="return-value"></a>Valor devuelto  
Una matriz de información de errores de la operación más reciente en el identificador de instrucción. La matriz consta de los siguientes campos:  
  
-   El código de error de SQLSTATE  
  
-   El código de error específico del controlador  
  
-   El mensaje de error específico del controlador  
  
Si no hay ningún error, o si no se establece el valor de SQLSTATE, los campos específicos del controlador tendrán el valor Null.  
  
## <a name="remarks"></a>Observaciones  
En la versión 2.0 de los [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], se agregó compatibilidad con PDO.  
  
## <a name="example"></a>Ejemplo  
En este ejemplo, la instrucción SQL tiene un error, que se indica a continuación.  
  
```php
<?php  
$conn = new PDO( "sqlsrv:server=(local) ; Database = AdventureWorks", "", "");  
$stmt = $conn->prepare('SELECT * FROM Person.Addressx');  
  
$stmt->execute();  
print_r ($stmt->errorInfo());  
?>  
```

## <a name="additional-odbc-messages"></a>Mensajes de ODBC adicionales

Cuando se produce una excepción, el controlador ODBC puede devolver más de un error para ayudar a diagnosticar problemas. Pero PDOStatement::errorInfo siempre muestra solo el primer error. En respuesta a este [informe de errores](https://bugs.php.net/bug.php?id=78196), [PDO::errorInfo](https://www.php.net/manual/en/pdo.errorinfo.php) y [PDOStatement::errorInfo](https://www.php.net/manual/en/pdostatement.errorinfo.php) se han actualizado para indicar que los controladores deben mostrar *al menos* los tres campos siguientes:
```
0   SQLSTATE error code (a five characters alphanumeric identifier defined in the ANSI SQL standard).
1   Driver specific error code.
2   Driver specific error message.
```

A partir de la versión 5.9.0, el comportamiento predeterminado de PDOStatement::errorInfo es mostrar los errores de ODBC adicionales, si están disponibles. Consulte [PDO::errorInfo](../../connect/php/pdo-errorinfo.md) para obtener más información.
  
## <a name="see-also"></a>Consulte también  
[Clase PDOStatement](../../connect/php/pdostatement-class.md)

[PDO::errorInfo](../../connect/php/pdo-errorinfo.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
