---
description: MSSQLSERVER_107
title: MSSQLSERVER_107 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 107 (Database Engine error)
ms.assetid: f33f514c-56aa-42e2-841b-e91244da90e2
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 9da64b573efcdce2adc52e0d7cd419d40ea8289e
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "88337661"
---
# <a name="mssqlserver_107"></a>MSSQLSERVER_107
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|107|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|P_NOCORRMATCH|  
|Texto del mensaje|El prefijo de columna '%.*ls' no coincide con un nombre de tabla o un nombre de alias utilizado en la consulta.|  
  
## <a name="explanation"></a>Explicación  
La lista de selección de la consulta contiene un asterisco (*) que se califica incorrectamente con un prefijo de columna. Este error se puede devolver en las condiciones siguientes:  
  
-   El prefijo de columna no se corresponde con ningún nombre de tabla o de alias utilizado en la consulta. Por ejemplo, la instrucción siguiente utiliza un nombre de alias (`T1`) como prefijo de columna, pero el alias no está definido en la cláusula FROM.  
  
    ```  
    SELECT T1.* FROM dbo.ErrorLog;  
    ```  
  
-   Se especifica un nombre de tabla como prefijo de columna cuando se proporciona un nombre de alias para la tabla en la cláusula FROM. Por ejemplo, la instrucción siguiente utiliza el nombre de tabla `ErrorLog` como prefijo de columna; sin embargo, la tabla tiene definido un alias (`T1`) en la cláusula FROM.  
  
    ```  
    SELECT ErrorLog.* FROM dbo.ErrorLog AS T1;  
    ```  
  
    Si se ha proporcionado un alias para un nombre de tabla en la cláusula FROM, solo puede utilizar el alias como prefijo de las columnas de la tabla.  
  
## <a name="user-action"></a>Acción del usuario  
Haga coincidir los prefijos de columna con los nombres de tabla o con los nombres de alias especificados en la cláusula FROM de la consulta. Por ejemplo, las instrucciones anteriores se pueden corregir de la forma siguiente:  
  
```  
SELECT T1.* FROM dbo.ErrorLog AS T1;  
```  
  
or  
  
```  
SELECT ErrorLog.* FROM dbo.ErrorLog;  
```  
  
## <a name="see-also"></a>Consulte también  
[MSSQLSERVER_4104](~/relational-databases/errors-events/mssqlserver-4104-database-engine-error.md)  
  
