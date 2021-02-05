---
title: PDOStatement::execute
description: Referencia de API de la función PDOStatement::execute en el controlador PDO_SQLSRV de Microsoft para PHP en SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: c2e80566-fa41-4918-8521-cf2e05374cbd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7910425a9abc5dfff0560a9c886e0b439af85d1b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206349"
---
# <a name="pdostatementexecute"></a>PDOStatement::execute
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Ejecuta una instrucción.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
bool PDOStatement::execute ([ $input ] );  
```  
  
#### <a name="parameters"></a>Parámetros  
*$input*: (opcional). Una matriz asociativa que contiene los valores de los marcadores de parámetros.  
  
## <a name="return-value"></a>Valor devuelto  
Se devuelve True si se ejecuta correctamente; de lo contrario, se devuelve False.  
  
## <a name="remarks"></a>Observaciones  
Las instrucciones ejecutadas con PDOStatement::execute deben prepararse primero con [PDO::prepare](../../connect/php/pdo-prepare.md). Vea [Direct Statement Execution and Prepared Statement Execution in the PDO_SQLSRV Driver (Ejecución de la instrucción preparada o directa en el controlador PDO_SQLSRV)](../../connect/php/direct-statement-execution-prepared-statement-execution-pdo-sqlsrv-driver.md) para obtener información sobre cómo especificar la ejecución de la instrucción preparada o directa.  
  
Todos los valores de la matriz de parámetros de entrada se tratan como valores de PDO::PARAM_STR.  
  
Si la instrucción preparada incluye marcadores de parámetros, debe llamar a PDOStatement::bindParam para enlazar las variables PHP a tales marcadores, o bien transmitir una matriz de valores de parámetro de solo entrada.  
  
En la versión 2.0 de los [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], se agregó compatibilidad con PDO.  
  
## <a name="example"></a>Ejemplo  
  
```  
<?php  
$database = "AdventureWorks";  
$server = "(local)";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$query = "select * from Person.ContactType";  
$stmt = $conn->prepare( $query );  
$stmt->execute();  
  
while ( $row = $stmt->fetch( PDO::FETCH_ASSOC ) ){  
   print "$row[Name]\n";  
}  
  
echo "\n";  
$param = "Owner";  
$query = "select * from Person.ContactType where name = ?";  
$stmt = $conn->prepare( $query );  
$stmt->execute(array($param));  
  
while ( $row = $stmt->fetch( PDO::FETCH_ASSOC ) ){  
   print "$row[Name]\n";  
}  
?>  
```  
  
> [!NOTE]
> Se recomienda utilizar cadenas como entradas cuando se vinculen valores a una [columna decimal o numérica](../../t-sql/data-types/decimal-and-numeric-transact-sql.md) para garantizar la precisión y la exactitud, ya que PHP tiene una precisión limitada para [números de punto flotante](https://php.net/manual/en/language.types.float.php). Lo mismo se aplica a las columnas de tipo bigint, especialmente cuando los valores están fuera del intervalo de un [entero](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md).

## <a name="see-also"></a>Consulte también  
[Clase PDOStatement](../../connect/php/pdostatement-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
