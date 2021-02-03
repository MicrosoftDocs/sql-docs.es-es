---
description: MSSQLSERVER_1904
title: MSSQLSERVER_1904 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1904 (Database Engine error)
ms.assetid: 2a35d57d-74e2-45a2-8f67-3f2e51d69712
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 198fe8af27532157397b32875882a072f26f6f4f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196477"
---
# <a name="mssqlserver_1904"></a>MSSQLSERVER_1904
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|1904|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|KEYCOUNT|  
|Texto del mensaje|%S_MSG '%.*ls' en la tabla '%.\*ls' tiene %d nombres de columna en la lista de claves %S_MSG. El límite máximo para la lista de columnas de clave de índice o estadísticas es %d.|  
  
## <a name="explanation"></a>Explicación  
La lista de columnas de clave para el índice o estadística especificados supera el número máximo permitido de columnas.  
  
## <a name="user-action"></a>Acción del usuario  
Modifique la lista de columnas de clave para que no incluya más columnas del número máximo especificado.  
  
Con los índices no clúster, considere utilizar la cláusula INCLUDE en la instrucción CREATE INDEX para agregar columnas al índice como columnas sin clave. Este método impide superar la limitación de tamaño de índice actual de 16 columnas de clave como máximo. Para más información, consulte [Create Indexes with Included Columns](~/relational-databases/indexes/create-indexes-with-included-columns.md).  
  
## <a name="see-also"></a>Consulte también  
[CREATE INDEX &#40;Transact-SQL&#41;](~/t-sql/statements/create-index-transact-sql.md)  
[CREATE STATISTICS &#40;Transact-SQL&#41;](~/t-sql/statements/create-statistics-transact-sql.md)  
  
