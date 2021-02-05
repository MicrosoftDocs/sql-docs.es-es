---
title: PDOStatement::bindColumn
description: Referencia de API de la función PDOStatement::bindColumn en el controlador PDO_SQLSRV de Microsoft para PHP en SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: bbdcea53-d23d-4769-89a0-95c7cf4d5390
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d9ed841aaf609178ee28b6340206421fa6d0eb81
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195223"
---
# <a name="pdostatementbindcolumn"></a>PDOStatement::bindColumn
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Enlaza una variable a una columna de un juego de resultados.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
bool PDOStatement::bindColumn($column, &$param[, $type[, $maxLen[, $driverdata ]]] );  
```  
  
#### <a name="parameters"></a>Parámetros  
$*column*: el número (mixto) de la columna (índice basado en 1) o el nombre de la columna del conjunto de resultados.  
  
&$*param*: el nombre mixto de la variable PHP a la que se enlazará la columna.  
  
$*type*: el tipo de datos opcional del parámetro, que se representa mediante una constante PDO::PARAM_*.  
  
$*maxLen*: valor entero opcional que no utilizan los controladores de Microsoft para PHP en SQL Server.  
  
$*driverdata*: parámetros mixtos opcionales del controlador. Por ejemplo, podría especificar PDO::SQLSRV_ENCODING_UTF8 para enlazar la columna a una variable como una cadena codificada en UTF-8.  
  
## <a name="return-value"></a>Valor devuelto  
Se devuelve el valor TRUE si la operación se realiza correctamente; de lo contrario, se devuelve FALSE.  
  
## <a name="remarks"></a>Observaciones  
En la versión 2.0 de los [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], se agregó compatibilidad con PDO.  
  
## <a name="example"></a>Ejemplo  
En este ejemplo se muestra cómo se puede enlazar una variable a una columna de un conjunto de resultados.  
  
```  
<?php  
$database = "AdventureWorks";  
$server = "(local)";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$query = "SELECT Title, FirstName, EmailAddress FROM Person.Contact where LastName = 'Estes'";  
$stmt = $conn->prepare($query);  
$stmt->execute();  
  
$stmt->bindColumn('EmailAddress', $email);  
while ( $row = $stmt->fetch( PDO::FETCH_BOUND ) ){  
   echo "$email\n";  
}  
?>  
```  
  
## <a name="see-also"></a>Consulte también  
[Clase PDOStatement](../../connect/php/pdostatement-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
