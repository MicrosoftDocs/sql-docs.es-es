---
title: PDOStatement::bindValue
description: Referencia de API de la función PDOStatement::bindValue en el controlador PDO_SQLSRV de Microsoft para PHP en SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 13bc4ece-420e-4887-8809-bf0705ddf126
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 40b696cf3cd8cc0c475a052d78966ee45b5f7b72
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195211"
---
# <a name="pdostatementbindvalue"></a>PDOStatement::bindValue
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Enlaza un valor a un marcador de posición con nombre o signo de interrogación en la instrucción SQL.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
bool PDOStatement::bindValue($parameter, $value[, $data_type]);  
```  
  
#### <a name="parameters"></a>Parámetros  
$*parameter*: identificador de parámetro (mixto). En una instrucción que use marcadores de posición con nombre, emplee un nombre de parámetro (:name). En una instrucción preparada mediante la sintaxis de signos de interrogación, es el índice basado en 1 del parámetro.
  
$*value*: valor (mixto) que se va a enlazar con el parámetro.  
  
$*data_type*: tipo de datos opcional (entero) representado por una constante PDO::PARAM_*. El valor predeterminado es PDO::PARAM_STR.  
  
## <a name="return-value"></a>Valor devuelto  
Se devuelve True si la operación se realiza correctamente; de lo contrario, False.  
  
## <a name="remarks"></a>Observaciones  
  
En la versión 2.0 de los [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], se agregó compatibilidad con PDO.  
  
## <a name="parameter-example"></a>Ejemplo de parámetro  
En este ejemplo se muestra que, una vez que se enlaza el valor de $contact, al cambiar el valor, no se modificará el valor transmitido en la consulta.  
  
```  
<?php  
$database = "AdventureWorks";  
$server = "(local)";  
$conn = new PDO("sqlsrv:server=$server ; Database = $database", "", "");  
  
$contact = "Sales Agent";  
$stmt = $conn->prepare("select * from Person.ContactType where name = ?");  
$stmt->bindValue(1, $contact);  
$contact = "Owner";  
$stmt->execute();  
  
while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {  
   print "$row[Name]\n\n";  
}  
  
$stmt = null;  
$contact = "Sales Agent";  
$stmt = $conn->prepare("select * from Person.ContactType where name = :contact");  
$stmt->bindValue(':contact', $contact);  
$contact = "Owner";  
$stmt->execute();  
  
while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {  
   print "$row[Name]\n\n";  
}  
?>  
```

> [!NOTE]
> Se recomienda utilizar cadenas como entradas cuando se vinculen valores a una [columna decimal o numérica](../../t-sql/data-types/decimal-and-numeric-transact-sql.md) para garantizar la precisión y la exactitud, ya que PHP tiene una precisión limitada para [números de punto flotante](https://php.net/manual/en/language.types.float.php). Lo mismo se aplica a las columnas de tipo bigint, especialmente cuando los valores están fuera del intervalo de un [entero](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md).

## <a name="decimal-input-example"></a>Ejemplo de entrada decimal  
Este ejemplo de código muestra cómo enlazar un valor decimal como un parámetro de entrada.  

```
<?php  
$database = "Test";  
$server = "(local)";  
$conn = new PDO("sqlsrv:server=$server ; Database = $database", "", "");  

// Assume TestTable exists with a decimal field 
$input = 9223372036854.80000;
$stmt = $conn->prepare("INSERT INTO TestTable (DecimalCol) VALUES (?)");
// by default it is PDO::PARAM_STR, rounding of a large input value may
// occur if PDO::PARAM_INT is specified
$stmt->bindValue(1, $input, PDO::PARAM_STR);
$stmt->execute();
```
  
## <a name="see-also"></a>Consulte también  
[Clase PDOStatement](../../connect/php/pdostatement-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
