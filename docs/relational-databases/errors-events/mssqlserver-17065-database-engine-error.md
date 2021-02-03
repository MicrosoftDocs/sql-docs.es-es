---
description: MSSQLSERVER_17065
title: MSSQLSERVER_17065 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17065 (Database Engine error)
ms.assetid: 63c2ba5a-be34-461e-bee1-03c25b396cd2
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: cfebe120fa4b51158483ea9b8fb504471657ff75
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196858"
---
# <a name="mssqlserver_17065"></a>MSSQLSERVER_17065
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|17065|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|SQLASSERT_BOTH|  
|Texto del mensaje|Aserción de SQL Server: Archivo: \<%s>, línea = %d Error de aserción = '%s' %s. Puede que este error esté relacionado con el tiempo de espera. Si el error persiste después de volver a ejecutar la instrucción, utilice DBCC CHECKDB para comprobar la integridad estructural de la base de datos, o bien reinicie el servidor para asegurarse de que las estructuras de datos en memoria no están dañadas.|  
  
## <a name="explanation"></a>Explicación  
Este error puede deberse a errores transitorios relacionados con el control de tiempo, o a que los datos en memoria o en disco estén dañados.  
  
## <a name="user-action"></a>Acción del usuario  
Vuelva a ejecutar la instrucción que hizo que se desencadenara la excepción. Si el error se debe a un evento relacionado con el control de tiempo, puede que el error no se repita. Si el problema persiste, ejecute DBCC CHECKDB para comprobar si el disco está dañado. Reinicie el servidor para asegurarse de que las estructuras de datos en memoria no están dañadas.  
  
## <a name="see-also"></a>Consulte también  
[DBCC CHECKDB &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
  
