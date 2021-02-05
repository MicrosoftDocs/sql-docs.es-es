---
title: PDOStatement::rowCount
description: Referencia de API de la función PDOStatement::rowCount en el controlador PDO_SQLSRV de Microsoft para PHP en SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 0569f26a-2376-4c20-8813-bd3c87d0ae9f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 765a3af1800e7a3a3a834130659637322a51e374
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179905"
---
# <a name="pdostatementrowcount"></a>PDOStatement::rowCount
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Devuelve el número de filas agregadas, eliminadas o cambiadas en la última instrucción.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
int PDOStatement::rowCount ();  
```  
  
## <a name="return-value"></a>Valor devuelto  
El número de filas agregadas, eliminadas o cambiadas.  
  
## <a name="remarks"></a>Observaciones  
Si la última instrucción SQL ejecutada mediante la clase PDOStatement asociado fue SELECT, el cursor PDO::CURSOR_FWDONLY devuelve -1. Un cursor PDO::CURSOR_SCROLLABLE devuelve el número de filas del conjunto de resultados.  
  
En la versión 2.0 de los [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], se agregó compatibilidad con PDO.  
  
## <a name="example"></a>Ejemplo  
En este ejemplo se muestran dos usos de rowCount. En el primero, se devuelve el número de filas que se agregaron a la tabla. En el segundo, se muestra que rowCount puede devolver el número de filas en un conjunto de resultados al especificar un cursor desplazable.  
  
```  
<?php  
$database = "Test";  
$server = "(local)";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$col1 = 'a';  
$col2 = 'b';  
  
$query = "insert into Table2(col1, col2) values(?, ?)";  
$stmt = $conn->prepare( $query );  
$stmt->execute( array( $col1, $col2 ) );  
print $stmt->rowCount();  
  
echo "\n\n";  
  
$con = null;  
$database = "AdventureWorks";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$query = "select * from Person.ContactType";  
$stmt = $conn->prepare( $query, array(PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL));  
$stmt->execute();  
print $stmt->rowCount();  
?>  
```  
  
## <a name="see-also"></a>Consulte también  
[Clase PDOStatement](../../connect/php/pdostatement-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
