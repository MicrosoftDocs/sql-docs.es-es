---
description: ELIMINAR, comando SQL
title: Comando DELETE-SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- DELETE [ODBC]
ms.assetid: 0d5bd477-626f-4f22-a05a-f531d9f8c5e7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 42c5ff48b0ac330c2a305650141ada2255629c1f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200512"
---
# <a name="delete---sql-command"></a>ELIMINAR, comando SQL
Marca los registros para su eliminación.  
  
 El controlador ODBC de Visual FoxPro admite la sintaxis nativa del lenguaje Visual FoxPro para este comando. Para obtener información específica del controlador, consulte la sección Comentarios.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
DELETE FROM [DatabaseName!]TableName  
   [WHERE FilterCondition1 [AND | OR FilterCondition2 ...]]  
```  
  
## <a name="arguments"></a>Argumentos  
 DESDE [ *nombreDeBaseDeDatos!*] *TableName*  
 Especifica la tabla en la que los registros se marcan para su eliminación.  
  
 *DatabaseName!* especifica el nombre de una base de datos que contiene la tabla si la base de datos contenedora no es la base de datos especificada con el origen de datos. Debe incluir el nombre de una base de datos que contenga la tabla si la base de datos no es la base de datos especificada con el origen de datos. Incluya el delimitador de signo de exclamación (!) después del nombre de la base de datos y antes del nombre de la tabla.  
  
 WHERE *FilterCondition1*[and &#124; or *FilterCondition2*...]  
 Especifica que Visual FoxPro marca solo determinados registros para su eliminación.  
  
 *FilterCondition* especifica los criterios que los registros deben cumplir para que se marquen para su eliminación. Puede incluir tantas condiciones de filtro como desee, conectarlas con el operador AND u OR. También puede usar el operador NOT para invertir el valor de una expresión lógica, o puede usar **Empty**() para comprobar si hay un campo vacío.  
  
## <a name="remarks"></a>Observaciones  
 Si SET DELETEd está establecido en ON, todos los comandos que incluyen un ámbito omiten los registros marcados para su eliminación.  
  
 DELETE: SQL usa el bloqueo de registros al marcar varios registros para su eliminación en tablas abiertas para el acceso compartido. Esto reduce la contención de registros en situaciones multiusuario, pero puede reducir el rendimiento. Para obtener el máximo rendimiento, abra la tabla para su uso exclusivo.  
  
## <a name="driver-remarks"></a>Notas del controlador  
 Cuando la aplicación envía la instrucción SQL de ODBC DELETE al origen de datos, el controlador ODBC de Visual FoxPro convierte el comando en el comando de eliminación de Visual FoxPro sin conversión.  
  
## <a name="see-also"></a>Consulte también  
 [Comando de eliminaciones de Set](../../odbc/microsoft/set-deleted-command.md)
