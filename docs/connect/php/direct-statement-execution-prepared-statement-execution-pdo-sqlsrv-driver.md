---
title: 'Instrucción directa: ejecución de la instrucción preparada en el controlador PDO_SQLSRV'
description: Aprenda a usar el atributo PDO::SQLSRV_ATTR_DIRECT_QUERY para la ejecución de la instrucción directa al usar el controlador de Microsoft PDO_SQLSRV para PHP en SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 05544ca6-1e07-486c-bf03-e8c2c25b3024
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4dc9177ed83e349b4cdd3d38da34285ee90a1f1f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045395"
---
# <a name="direct-statement-execution-and-prepared-statement-execution-in-the-pdo_sqlsrv-driver"></a>Direct Statement Execution and Prepared Statement Execution in the PDO_SQLSRV Driver (Ejecución de la instrucción preparada o directa en el controlador PDO_SQLSRV)
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

En este tema se describe el uso del atributo PDO::SQLSRV_ATTR_DIRECT_QUERY para especificar la ejecución de la instrucción directa en lugar de la acción predeterminada, que es la ejecución de la instrucción preparada. El uso de una instrucción preparada puede dar lugar a un mejor rendimiento si la instrucción se ejecuta más de una vez mediante el enlace de parámetros.  
  
## <a name="remarks"></a>Observaciones  
Si desea enviar una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] directamente al servidor sin preparación de instrucciones para el controlador, puede establecer el atributo PDO::SQLSRV_ATTR_DIRECT_QUERY con [PDO::setAttribute](../../connect/php/pdo-setattribute.md) (o como una opción del controlador pasada a [PDO::__construct](../../connect/php/pdo-construct.md)) o cuando se llama a [PDO::prepare](../../connect/php/pdo-prepare.md). De forma predeterminada, el valor de PDO::SQLSRV_ATTR_DIRECT_QUERY es False (se usa la ejecución de la instrucción preparada).  
  
Si usa [PDO::query](../../connect/php/pdo-query.md), es posible que desee la ejecución directa. Antes de llamar a [PDO::query](../../connect/php/pdo-query.md), llame a [PDO::setAttribute](../../connect/php/pdo-setattribute.md) y establezca PDO::SQLSRV_ATTR_DIRECT_QUERY en True.  Cada llamada a [PDO::query](../../connect/php/pdo-query.md) puede ejecutarse con un valor diferente para PDO::SQLSRV_ATTR_DIRECT_QUERY.  
  
Si usa [PDO::prepare](../../connect/php/pdo-prepare.md) y [PDOStatement::execute](../../connect/php/pdostatement-execute.md) para ejecutar una consulta varias veces con parámetros enlazados, la ejecución de la instrucción preparada optimiza la ejecución de la consulta repetida.  En esta situación, llame a [PDO::prepare](../../connect/php/pdo-prepare.md) con PDO::SQLSRV_ATTR_DIRECT_QUERY establecido en False en el parámetro de matriz de opciones de controlador. Cuando sea necesario, puede ejecutar instrucciones preparadas con PDO::SQLSRV_ATTR_DIRECT_QUERY establecido en False.  
  
Después de llamar a [PDO::prepare](../../connect/php/pdo-prepare.md), el valor de PDO::SQLSRV_ATTR_DIRECT_QUERY no puede cambiar al ejecutar la consulta preparada.  
  
Si una consulta requiere el contexto que se estableció en una consulta anterior, ejecute las consultas con PDO::SQLSRV_ATTR_DIRECT_QUERY establecido en True. Por ejemplo, si usa tablas temporales en las consultas, PDO::SQLSRV_ATTR_DIRECT_QUERY debe establecerse en True.  
  
En el ejemplo siguiente se muestra que cuando se requiere el contexto de una instrucción anterior, es necesario establecer PDO::SQLSRV_ATTR_DIRECT_QUERY en true. En este ejemplo se usan tablas temporales, que solo están disponibles para las siguientes instrucciones del programa cuando las consultas se ejecutan directamente.  
  
> [!NOTE]
> Si la consulta va a invocar un procedimiento almacenado y las tablas temporales se utilizan en este procedimiento almacenado, use [PDO::exec](../../connect/php/pdo-exec.md) en su lugar.

```  
<?php  
   $conn = new PDO('sqlsrv:Server=(local)', '', '');  
   $conn->setAttribute(constant('PDO::SQLSRV_ATTR_DIRECT_QUERY'), true);  
  
   $stmt1 = $conn->query("DROP TABLE #php_test_table");  
  
   $stmt2 = $conn->query("CREATE TABLE #php_test_table ([c1_int] int, [c2_int] int)");  
  
   $v1 = 1;  
   $v2 = 2;  
  
   $stmt3 = $conn->prepare("INSERT INTO #php_test_table (c1_int, c2_int) VALUES (:var1, :var2)");  
  
   if ($stmt3) {  
      $stmt3->bindValue(1, $v1);  
      $stmt3->bindValue(2, $v2);  
  
      if ($stmt3->execute())  
         echo "Execution succeeded\n";       
      else  
         echo "Execution failed\n";  
   }  
   else  
      var_dump($conn->errorInfo());  
  
   $stmt4 = $conn->query("DROP TABLE #php_test_table");  
?>  
```  
  
## <a name="see-also"></a>Consulte también  
[Guía de programación para los controladores de Microsoft para PHP para SQL Server](../../connect/php/programming-guide-for-php-sql-driver.md)
  
