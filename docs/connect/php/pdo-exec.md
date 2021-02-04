---
title: PDO::exec
description: Referencia de API de la función PDO::exec en el controlador PDO_SQLSRV de Microsoft para PHP en SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 359a87c6-c13a-4518-8f23-a922e7f3b171
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ad79792d21e9b097f6aee681b1097c8aacfa44fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202019"
---
# <a name="pdoexec"></a>PDO::exec
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Prepara y ejecuta una instrucción SQL en una única llamada a una función, y devuelve el número de filas a las que afecta la instrucción.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
int PDO::exec ($statement)  
```  
  
#### <a name="parameters"></a>Parámetros  
*$statement*: una cadena que contiene la instrucción SQL que se ejecutará.  
  
## <a name="return-value"></a>Valor devuelto  
Un valor entero que notifica el número de filas afectadas.  
  
## <a name="remarks"></a>Observaciones  
Si *$statement* contiene varias instrucciones SQL, solo se notifica el recuento de filas afectadas de la última instrucción.  
  
PDO::exec no devuelve los resultados de una instrucción SELECT.  
  
Los siguientes atributos afectan al comportamiento de PDO::exec:  
  
-   PDO::ATTR_DEFAULT_FETCH_MODE  
  
-   PDO::SQLSRV_ATTR_ENCODING  
  
-   PDO::SQLSRV_ATTR_QUERY_TIMEOUT  
  
Para obtener más información, consulte [PDO::setAttribute](../../connect/php/pdo-setattribute.md). 
  
En la versión 2.0 de los [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], se agregó compatibilidad con PDO.  
  
## <a name="example"></a>Ejemplo  
En este ejemplo se eliminan las filas de Table1 que incluyen "xxxyy" en col1. En el ejemplo se informa de cuántas filas se han eliminado.  
  
```  
<?php  
   $c = new PDO( "sqlsrv:server=(local)");  
  
   $c->exec("use Test");  
   $ret = $c->exec("delete from Table1 where col1 = 'xxxyy'");  
   echo $ret;  
?>  
```  
  
## <a name="see-also"></a>Consulte también  
[Clase PDO](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
