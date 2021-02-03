---
description: MSSQLSERVER_107
title: MSSQLSERVER_107 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 107 (Database Engine error)
ms.assetid: f33f514c-56aa-42e2-841b-e91244da90e2
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: dcd1043ccfd56b03b13731a8a2b0b2772bf13163
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197328"
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
  
