---
description: MSSQL_ENG004929
title: MSSQL_ENG004929 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG004929 error
ms.assetid: 1d9b1d88-1fbf-4089-b392-687d3b0220ca
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 246c01776e8af4ad31666653387eebf20e2bb8b9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475796"
---
# <a name="mssql_eng004929"></a>MSSQL_ENG004929
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Detalles del mensaje  
  
|Atributo|Value|  
|-|-|  
|Nombre de producto|SQL Server|  
|Id. de evento|4929|  
|Origen de eventos|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nombre simbólico||  
|Texto del mensaje|No se puede modificar el %1! '%.*ls!' porque se está publicando para replicación.|  
  
## <a name="explanation"></a>Explicación  
 Normalmente este error se produce si intenta quitar la restricción de clave principal en una tabla que está publicada para replicación transaccional. La replicación transaccional requiere una clave principal para cada tabla publicada; por tanto, no se puede quitar la restricción.  
  
## <a name="user-action"></a>Acción del usuario  
 Para quitar la restricción, primero quite el artículo asociado con la tabla. Para más información, vea [Agregar y quitar artículos de publicaciones existentes](../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md). Si se produce este error en una base de datos que no está replicada, ejecute [sp_removedbreplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) para asegurarse de que los objetos de la base de datos no están marcados como replicados.  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de errores y eventos &#40;replicación&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
